
# Electronics Cheatsheet

## Basics

### Common terms

- Ampere (A): measure of current, 1A = 1C/sec
- Charge: measured in coulumbs (C). 1C = the charge of 6.25x10^18 electrons
- Conductors: materials with low resistance, eg. copper, silver, gold
- Current (I): the amount of electrons moving past a point every second
- Electricity: the flow of electrons
- Ground: zero voltage. A.k.a. 0V, minus terminal, earth, GND. Usually color-coded black.
- Insulators: materials with a high resistance, eg. clay, plastics
- Ohm (greek omega symbol): measure of resistance
- Ohm's law: V = I * R (volts = amps * resistance)
- Open circuit vs closed circuit: circuit is open if electricity cannot flow through a device.
- Power (P): amount of work a circuit is doing.
- Short circuit: an umwanted conductive bridge.
- Resistance (R): how tightly materials hold onto electrons. Measured in ohms
- Volt (V): measure of voltage. 1V will push 1A through 1 ohm of resistance
- Voltage (E): measure of pressure pushing the electrons. Voltage refers to a difference in electrical potential between two parts of a circuit. Voltage = ohms * amps.
- Watt (W): measure of power, watts = volts * amps

## Tools

### Using a multimeter

- Voltage is measured relatively, between two points in a circuit. Use the multimeter like a stethoscope. Set to volts.
- Current measures the flow in a circuit - i.e. it must past through the meter. Set to milliamps.
- The black wire plugs into the socket usually labeled "COM".
- The red wire plugs into the socket usually labeled 'V" or "volts".
- If the meter doesn't support autoranging, the numbered range positions mean "no higher than".

## Circuits

- Series: components connected one after another
  - Total resistance is the sum of each resistance
  - Total capacitance = (C1 * C2) / (C1 + C2)
- Parallel: components connected side by side
  - Total resistance = (R1 * R2) / (R1 + R2)
  - Total capacitance is the sum of each capacitance

### How to limit current through a component with a resistor

- Example starting circuit: -5V at 0.02A power supply -> LED (only needs 2V) -> +5V.
- Resistor therefore needs to drop 3V (5V - 2V).
- Use Ohm's law to calculate resistor size: E=IR, 3V = 0.02A * R. Solve for R: 155 Ohms.
- Find resistor equal to or greater than solution: 220 Ohms.
- New current limited circuit: -5V -> LED -> Resistor -> +5V.
- It does not matter which side of the circuit the resistor goes on (before or after the LED).

## Gates

Gates are logic circuits. They take binary input and produce binary output. 0 = no electricity, 1 = positive electricity.
- AND: both 1 inputs = 1 output
- OR: one or more 1 inputs = 1 output
- NOT: one 0 input = 1 output (known as inverter circuit)
- NAND: one or more 0 inputs = 1 output
- NOR: all 0 inputs = 1 output

## Schematic diagrams

Shows how each component is connected to each other. Each component type has a unique symbol and name (usually 1-2 letters).

## Components

### LEDs

- Longer wire is for the positive voltage
- Voltage difference between wires must not exceed manufacturer limit
- The current between wires must not exceed manufacturer limit

### Potentiometers

- Typically look like knobs
- Used to vary resistance

### Resistors

- Create a resistance in electron flow, like stepping on a water hose.
- Convert 1st and 2nd stripe by color value, then add color zeroes.
- Last band represents accuracy: silver is 10%, gold is 5%.
- When resistors are in series, electricy has to pass through each of them to reach the other, so resistance rises.
- When resistors are in parallel, electricty can flow through both, so resistance drops.

| Color | Value |
| - | - |
| Black | 0 |
| Brown | 1 |
| Red | 2 |
| Orange | 3 |
| Yellow | 4 |
| Green | 5 |
| Blue | 6 |
| Violet | 7 |
| Gray | 8 |
| White | 9 |

| Color | Zeroes |
| - | - |
| Black | 0 |
| Brown | 1 |
| Red | 2 |
| Orange | 3 |
| Yellow | 4 |
| Green | 5 |
| Blue | 6 |
| Violet | 7 |
| Gray | 8 |
| White | 9 |

### Capacitors

- Like a mini rechargable battery
- Amount of charge they can hold is measured in Farads (F)
- 2 common types:
  - Ceramic: brown with a disc shape. Non-polarized
  - Electrolytic: Cylinder shape. Polarized
 
### Diodes

- One way street for electrons
- Cylinder shaped. Electrons flow from cathode to anode end. Cathode end is marked with a band
        - 3 common types are diodes, zener diodes, and light emitting diodes
        - Zener diodes: Set voltage rating, which allows flow when voltage is exceeded
        - Light emitting diodes (LED) lights up with flow. No band to indicate flow, but the longer pin is the anode

### Transistors

- Used for switching and amplifying
- 2 common types are PNP and NPN. both have 3 pins: emitter, base, and collector

### Integrated circuits

- Usually referred to as chips
- Many different types
  - Logic circuits: usually contain logic gates
  - Comparators: compare input and give an output
  - Operational amplifiers: amplifiers
  - Timers

### Switches

- Connects and disconnects a circuit
- 3 common configurations:
  - SPST: Single pole, single throw. Two terminal switch
  - SPDT: 3 terminal switch. Connects one terminal to either of the other
  - DPDT: Double pole, double throw. 6 terminal switch, connects a pair of terminals to either of the other two pairs

## Units and formulas

| Ohms | Expressed as | Abbreviated as |
| - | - | - |
| 1000 ohms | 1 kilohm | 1K $\\Omega$ or 1K |
| 10000 ohms | 10 kilohm | 10K $\\Omega$ or 10K |
| 100000 ohms | 1 kilohm | 100K $\\Omega$ or 100K |
| 1000000 ohms | 1 kilohm | 1M $\\Omega$ or 1M |
| 10000000 ohms | 1 kilohm | 10M $\\Omega$ or 10M |

| Volts | Expresses as | Abbreviated as |
| - | - | - |
| 0.001 volts | 1 millivolt | 1mV |
| 0.01 volts | 10 millivolt | 10mV |
| 0.1 volts | 100 millivolt | 100mV |
| 1 volt | 10000 millivolt | 1V |

| Amps | Expresses as | Abbreviated as |
| - | - | - |
| 0.001 amps | 1 milliamp | 1mA |
| 0.01 amps | 10 milliamp | 10mA |
| 0.1 amps | 100 milliamp | 100mA |
| 1 amp | 1 amp | 1A |

| Watts | Expresses as | Abbreviated as |
| - | - | - |
| 0.001 watts | 1 milliwatt | 1mW |
| 0.01 watts | 10 milliwatt | 10mW |
| 0.1 watts | 100 milliwatt | 100mW |
| 1 watts | 1 watt | 1W |
| 1000 watts | 1 kilowatt | 1KW |
| 1000000 watts | 1 megawatt | 1MW |

