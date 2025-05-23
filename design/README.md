## The aeon-ttl design

Words.

## Build of Materials

An estimate of the first-pass in the design is thus.

| Item # |     Component     |     Part Number     | Quantity |                           Description                           | Approx. Unit Cost (USD) | Approx. Total Cost (USD) |
|:------:|:-----------------:|:-------------------:|:--------:|:---------------------------------------------------------------:|:-----------------------:|:------------------------:|
| 1      | SRAM (4-bit)      | 2114N or equivalent | 1        | 1024x4-bit SRAM (using 16 addresses)                            | $2.50                   | $2.50                    |
| 2      | 4-bit Counter     | 74LS161             | 1        | Synchronous 4-bit binary counter                                | $0.80                   | $0.80                    |
| 3      | 4-bit Register    | 74LS173             | 1        | 4-bit D-type register                                           | $1.00                   | $1.00                    |
| 4      | 555 Timer         | NE555P              | 1        | Timer IC for clock generation                                   | $0.50                   | $0.50                    |
| 5      | NAND Gate IC      | 74LS00              | 1        | Quad 2-input NAND gate for control logic                        | $0.60                   | $0.60                    |
| 6      | AND Gate IC       | 74LS08              | 1        | Quad 2-input AND gate for control logic                         | $0.60                   | $0.60                    |
| 7      | Inverter IC       | 74LS04              | 1        | Hex inverter for control logic                                  | $0.50                   | $0.50                    |
| 8      | D Flip-Flop       | 74LS74              | 1        | Dual D flip-flop for read/write toggle                          | $0.60                   | $0.60                    |
| 9      | SPST Switch       | Generic             | 5        | Single-pole single-throw switches (4 for data input, 1 for R/W) | $0.30                   | $1.50                    |
| 10     | LED               | Generic (Red)       | 4        | Indicator LEDs for 4-bit data output                            | $0.10                   | $0.40                    |
| 11     | Resistor (10kΩ)   | Generic             | 5        | Pull-down resistors for switches                                | $0.05                   | $0.25                    |
| 12     | Resistor (330Ω)   | Generic             | 4        | Current-limiting resistors for LEDs                             | $0.05                   | $0.20                    |
| 13     | Capacitor (0.1µF) | Generic             | 2        | Decoupling capacitors for ICs                                   | $0.10                   | $0.20                    |
| 14     | Capacitor (10µF)  | Generic             | 1        | Timing capacitor for 555 timer                                  | $0.15                   | $0.15                    |
| 15     | Resistor (1kΩ)    | Generic             | 2        | Timing resistors for 555 timer                                  | $0.05                   | $0.10                    |
| 16     | Power Supply      | Generic             | 1        | 5V DC power supply or regulator (e.g., 7805)                    | $1.00                   | $1.00                    |
| 17     | Breadboard        | Generic             | 1        | Prototyping board for assembly                                  | $5.00                   | $5.00                    |
| 18     | Jumper Wires      | Generic             | 50       | Connecting wires for breadboard                                 | $0.05                   | $2.50                    |

Words.