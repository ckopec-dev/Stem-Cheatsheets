
# Atari Basic Sounds

In Atari BASIC, musical notes are produced with the SOUND statement:

SOUND channel, pitch, distortion, volume

The pitch value (0–255) determines the note frequency. Lower numbers produce higher notes, and higher numbers produce lower notes.

### Common Atari BASIC note values

| Note          | Pitch Value |
| ------------- | ----------- |
| C5            | 121         |
| B4            | 128         |
| A#4           | 135         |
| A4            | 144         |
| G#4           | 152         |
| G4            | 161         |
| F#4           | 171         |
| F4            | 181         |
| E4            | 192         |
| D#4           | 203         |
| D4            | 215         |
| C#4           | 228         |
| C4 (Middle C) | 242         |
| B3            | 255         |

### One octave lower

| Note | Pitch Value |
| ---- | ----------- |
| C3   | 114         |
| D3   | 102         |
| E3   | 91          |
| F3   | 86          |
| G3   | 76          |
| A3   | 68          |
| B3   | 64          |
| C4   | 60          |

### What the SOUND parameters mean

| Parameter | Meaning                |
| --------- | ---------------------- |
| 0         | Audio channel (0–3)    |
| 242       | Pitch (Middle C)       |
| 10        | Distortion / tone type |
| 8         | Volume (0–15)          |

### Useful distortion values

| Distortion | Sound                           |
| ---------- | ------------------------------- |
| 10         | Pure musical tone (most common) |
| 12         | Brighter tone                   |
| 8          | Buzzy tone                      |
| 6          | Noise / percussion-like         |




