## Simplest of them all

### References

* RAM[7400](https://github.com/emotional-toys/RAM7400)
* ideal-game-[ttl](https://github.com/emotional-toys/ideal-game-ttl)

### The state-determinate system

_Summary_

* Handles 4-bit data (D0–D3) with 8-bit addressing (A0–A7) using a single 2114 RAM (1K x 4-bit).
* Uses a 1 kHz clock from a 555 timer, pausable via an SPST switch for debugging.
* Features SPST switches for 8-bit address (A0–A7), 4-bit data (D0–D3), and read/write control.
* Displays 4-bit data on LEDs (input during write, output during read).
* Uses a LOAD pushbutton to trigger read/write operations.
* Uses 74LS00 for debouncing the LOAD button (no 74LS132 available).
* Minimizes ICs: 2114, 74LS244, 555 timer, 74LS00 (4 ICs total).

Key Changes from [this](/design/PROCESS.md)

* Reverts to single 2114 RAM for 4-bit data (D0–D3), as you specified only enough SPST switches for 4-bit data.
* Adds SPST pause switch for the 555 timer (controls reset pin).
* Continues using 74LS00 for debouncing, with the same circuit as before.
* Uses eight SPST switches for addressing (A0–A7), four SPST switches for data (D0–D3), one SPST for read/write, and one pushbutton for LOAD.
* 74LS244 buffers only 4-bit data (uses half the IC, leaving the second buffer set unused).

_Build of materials_

|      Component      |  Quantity |              Description              |                        Notes                       |
|:-------------------:|:---------:|:-------------------------------------:|:--------------------------------------------------:|
| 2114 RAM            | 1         | 1K x 4-bit static RAM (18-pin DIP)    | Stores 4-bit data (D0–D3)                          |
| 74LS244             | 1         | Octal buffer/line driver (20-pin DIP) | Buffers 4-bit data (D0–D3)                         |
| 555 Timer           | 1         | Timer IC (8-pin DIP)                  | Generates 1 kHz clock, pausable                    |
| 74LS00              | 1         | Quad 2-input NAND gate (14-pin DIP)   | Debounces LOAD button, generates control signals   |
| SPST Switches       | 14        | Single-pole single-throw switches     | 8 address (A0–A7), 4 data (D0–D3), 1 R/W', 1 pause |
| Pushbutton (LOAD)   | 1         | Momentary SPST pushbutton             | Triggers read/write operation                      |
| LEDs                | 4         | Standard LEDs (e.g., 5mm, red)        | Display 4-bit data (D0–D3)                         |
| Resistors (10 kΩ)   | 14        | Pull-down/pull-up resistors           | 8 address, 4 data, 1 R/W', 1 pause, 2 debounce     |
| Resistors (220 Ω)   | 4         | Current-limiting resistors for LEDs   | Limit LED current to ~20 mA                        |
| Resistor (1 kΩ)     | 1         | For 555 timer (R1)                    | Sets 1 kHz clock frequency                         |
| Resistor (680 kΩ)   | 1         | For 555 timer (R2)                    | Sets 1 kHz clock frequency                         |
| Capacitor (1 µF)    | 1         | For 555 timer                         | Sets 1 kHz clock frequency                         |
| Capacitor (0.1 µF)  | 3         | Decoupling (2) and debounce (1)       | For U1, U2/U3/U4, and LOAD button                  |
| Capacitor (0.01 µF) | 1         | For 555 timer control                 | Stabilizes timer operation                         |
| Power Supply        | 1         | +5V DC power supply                   | Powers all ICs and LEDs                            |

_Notes_

For KiCad, the DigiKey site has the 2114 listed as MD2114AL3. Models are [here](https://github.com/cartheur-forks/digikey-kicad-library).