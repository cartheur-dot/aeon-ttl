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

Total Estimated Cost: $16.90.

### Adding base-5 to the mix

* IC1: 2114 SRAM (1024x4-bit, 18-pin DIP).
* IC2: 74LS173 (4-bit D-type register).
* IC3: NE555 (timer for 1 kHz clock).
* IC4: 74LS00 (quad 2-input NAND).
* IC5: 74LS08 (quad 2-input AND).
* IC6: 74LS04 (hex inverter).
* IC7–IC10: 74LS47 (BCD-to-7-segment decoders, 4 units).
* IC11–IC12: 74LS157 (quad 2-to-1 multiplexers, 2 units).
* IC13: 27C16 (2Kx8 ROM for base-5 conversion).
* SW1–SW8: SPST switches (address input, A0–A7).
* SW9–SW12: SPST switches (data input, D0–D3).
* SW13: SPST switch (read/program, R/W).
* SW14: SPST switch (address/data display select).
* LED1–LED12: LEDs (8 yellow for address, 4 orange for data).
* DISP1–DISP4: Common-anode 7-segment displays.
* R1–R13: 10 kΩ resistors (pull-down for switches).
* R14–R25: 330 Ω resistors (LED current-limiting).
* R26–R27: 1 kΩ resistors (555 timer).
* C1: 0.68 µF capacitor (555 timer for 1 kHz).
* C2–C4: 0.1 µF capacitors (decoupling for ICs).
* C5: 10 µF capacitor (555 timer).
* PWR: 5V DC power supply.

|                        Components                       | On-Hand |
|:-------------------------------------------------------:|:-------:|
| IC1: 2114 SRAM (1024x4-bit, 18-pin DIP).                |    X    |
| IC2: 74LS173 (4-bit D-type register).                   |    X    |
| IC3: NE555 (timer for 1 kHz clock).                     |    X    |
| IC4: 74LS00 (quad 2-input NAND).                        |    X    |
| IC5: 74LS08 (quad 2-input AND).                         |    X    |
| IC6: 74LS04 (hex inverter).                             |    X    |
| IC7–IC10: 74LS47 (BCD-to-7-segment decoders, 4 units).  |         |
| IC11–IC12: 74LS157 (quad 2-to-1 multiplexers, 2 units). |         |
| IC13: 27C16 (2Kx8 ROM for base-5 conversion).           |    X    |
| SW1–SW8: SPST switches (address input, A0–A7).          |    X    |
| SW9–SW12: SPST switches (data input, D0–D3).            |    X    |
| SW13: SPST switch (read/program, R/W).                  |    X    |
| SW14: SPST switch (address/data display select).        |    X    |
| LED1–LED12: LEDs (8 yel for address, 4 oran for data).  |    X    |
| DISP1–DISP4: Common-anode 7-segment displays.           |    X    |
| R1–R13: 10 kΩ resistors (pull-down for switches).       |    X    |
| R14–R25: 330 Ω resistors (LED current-limiting).        |    X    |
| R26–R27: 1 kΩ resistors (555 timer).                    |    X    |
| C1: 0.68 µF capacitor (555 timer for 1 kHz).            |         |
| C2–C4: 0.1 µF capacitors (decoupling for ICs).          |    X    |
| C5: 10 µF capacitor (555 timer).                        |    X    |
| PWR: 5V DC power supply.                                |    X    |