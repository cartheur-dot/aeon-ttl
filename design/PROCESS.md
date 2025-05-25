# 8-Bit Addressing, 4-Bit Data SRAM Interface Schematic and ROM Contents

This document describes a schematic for a digital logic circuit that interfaces an 8-bit address, 4-bit data system with SRAM, using SPST switches for input, LEDs for state indication, a 1 kHz clock, a read/program switch, and a four 7-segment display for base-5 output. It also specifies the ROM contents for binary-to-base-5 conversion to drive the display.

## Design Specifications
- **Addressing**: 8-bit address bus (256 locations) via 8 SPST switches.
- **Data**: 4-bit data bus via 4 SPST switches.
- **RAM**: 4-bit SRAM (e.g., 2114, using 256 of 1024 locations).
- **Indicators**: 12 LEDs (8 for address, 4 for data) to show binary state (0 or 1).
- **Display**: Four 7-segment displays showing address (0–255) or data (0–15) in base-5, selected by an SPST switch.
- **Control**: Read/program SPST switch for read/write operations.
- **Clock**: 1 kHz from a 555 timer for synchronous operation.

## Schematic Description

### Components
From the updated Bill of Materials (BoM):

| Item | Component | Part Number | Quantity | Description |
|------|-----------|-------------|----------|-------------|
| IC1 | SRAM (4-bit) | 2114N | 1 | 1024x4-bit SRAM (using 256 addresses) |
| IC2 | 4-bit Register | 74LS173 | 1 | 4-bit D-type register for data |
| IC3 | 555 Timer | NE555P | 1 | Timer IC for 1 kHz clock |
| IC4 | NAND Gate IC | 74LS00 | 1 | Quad 2-input NAND for control logic |
| IC5 | AND Gate IC | 74LS08 | 1 | Quad 2-input AND for control logic |
| IC6 | Inverter IC | 74LS04 | 1 | Hex inverter for control logic |
| IC7–IC10 | BCD-to-7-Segment Decoder | 74LS47 | 4 | Decoders for base-5 display |
| IC11–IC12 | Multiplexer | 74LS157 | 2 | Quad 2-to-1 multiplexers for address/data display switch |
| IC13 | ROM | 27C16 | 1 | 2Kx8 ROM for binary-to-base-5 conversion |
| SW1–SW8 | SPST Switch | Generic | 8 | Address input (A0–A7) |
| SW9–SW12 | SPST Switch | Generic | 4 | Data input (D0–D3) |
| SW13 | SPST Switch | Generic | 1 | Read/program (R/W) |
| SW14 | SPST Switch | Generic | 1 | Address/data display select |
| LED1–LED12 | LED (Red) | Generic | 12 | LEDs for 8 address, 4 data lines |
| R1–R13 | Resistor (10 kΩ) | Generic | 13 | Pull-down resistors for switches |
| R14–R25 | Resistor (330 Ω) | Generic | 12 | Current-limiting resistors for LEDs |
| C1 | Capacitor (0.68 µF) | Generic | 1 | Timing capacitor for 555 timer (1 kHz) |
| C2–C4 | Capacitor (0.1 µF) | Generic | 3 | Decoupling capacitors for ICs |
| C5 | Capacitor (10 µF) | Generic | 1 | Timing capacitor for 555 timer |
| R26–R27 | Resistor (1 kΩ) | Generic | 2 | Timing resistors for 555 timer |
| PWR | Power Supply | Generic | 1 | 5V DC power supply |
| DISP1–DISP4 | 7-Segment Display | Generic | 4 | Common-anode displays for base-5 output |

**Total Estimated Cost**: ~$31.50 (based on 2025 prices from DigiKey/Mouser).

### Connections

1. **Power and Ground**:
   - Connect Vcc (5V) and GND to all ICs:
     - **IC1 (2114)**: Pin 18 (Vcc), Pin 9 (GND).
     - **IC2 (74LS173)**: Pin 16 (Vcc), Pin 8 (GND).
     - **IC3 (NE555)**: Pin 8 (Vcc), Pin 1 (GND).
     - **IC4 (74LS00)**: Pin 14 (Vcc), Pin 7 (GND).
     - **IC5 (74LS08)**: Pin 14 (Vcc), Pin 7 (GND).
     - **IC6 (74LS04)**: Pin 14 (Vcc), Pin 7 (GND).
     - **IC7–IC10 (74LS47)**: Pin 16 (Vcc), Pin 8 (GND).
     - **IC11–IC12 (74LS157)**: Pin 16 (Vcc), Pin 8 (GND).
     - **IC13 (27C16)**: Pin 24 (Vcc), Pin 12 (GND).
   - Place 0.1 µF capacitors (C2–C4) between Vcc and GND near IC1, IC2, and IC13.

2. **Address Input (SW1–SW8)**:
   - Connect 8 SPST switches (SW1–SW8) to 5V (ON = 1) or GND via 10 kΩ resistors (R1–R8) for A0–A7.
   - Outputs to:
     - **IC1 (2114)**: A0–A7 (Pins 1–8).
     - **LED1–LED8**: Anodes via 330 Ω resistors (R14–R21); cathodes to GND.
     - **IC11–IC12 (74LS157)**: Inputs A0–A7 (IC11: Pins 2, 5, 11, 14; IC12: Pins 2, 5, 11, 14).

3. **Data Input/Output (SW9–SW12, IC2, LED9–LED12)**:
   - Connect 4 SPST switches (SW9–SW12) to 5V or GND via 10 kΩ resistors (R9–R12) for D0–D3.
   - Outputs to **IC2 (74LS173)**: D0–D3 (Pins 3, 4, 5, 6).
   - IC2 Q0–Q3 (Pins 14, 13, 12, 11) to:
     - **IC1 (2114)**: D0–D3 (Pins 10–13).
     - **LED9–LED12**: Anodes via 330 Ω resistors (R22–R25); cathodes to GND.
     - **IC11 (74LS157)**: Inputs B0–B3 (Pins 3, 6, 12, 15).
   - IC2 clock (Pin 7) to IC3 (NE555) Pin 3.
   - Tie IC2 OE1, OE2 (Pins 9, 10) to GND; clear (Pin 15) to 5V.

4. **Clock (IC3)**:
   - Configure **IC3 (NE555)** in astable mode (~1 kHz):
     - Pin 2 (TRIG), Pin 6 (THR) to C1 (0.68 µF) to GND.
     - Pin 6 to Pin 7 (DISCH) via R26 (1 kΩ).
     - Pin 7 to 5V via R27 (1 kΩ).
     - Pin 5 (CONT) to C5 (10 µF) to GND.
     - Pin 3 (OUT) to IC2 Pin 7 and control logic.
   - Frequency: f ≈ 1.44 / ((R26 + 2×R27) × C1) ≈ 1 kHz.

5. **Control Logic (IC4, IC5, IC6, SW13)**:
   - Connect **SW13** (read/program) to 5V (read = 1) or GND via R13 (program = 0).
   - **Write Enable (WE)**:
     - SW13 to **IC4 (74LS00)** Pin 1.
     - IC3 Pin 3 to IC4 Pin 2.
     - IC4 Pin 3 to **IC1 (2114)** WE (Pin 17, active low).
   - **Output Enable (OE)**:
     - SW13 to **IC6 (74LS04)** Pin 1.
     - IC6 Pin 2 to **IC5 (74LS08)** Pin 1.
     - IC3 Pin 3 to IC5 Pin 2.
     - IC5 Pin 3 to **IC1 (2114)** OE (Pin 15, active low).
   - **Chip Enable (CE)**: Tie IC1 Pin 16 to GND (always active).

6. **Display (IC7–IC10, IC11–IC12, IC13, DISP1–DISP4, SW14)**:
   - **Address/Data Select**:
     - **SW14** to 5V (address = 1) or GND (data = 0) via 10 kΩ resistor.
     - SW14 to **IC11–IC12 (74LS157)** select pins (Pin 1).
   - **Address Inputs**:
     - SW1–SW8 (A0–A7) to IC11–IC12 inputs A0–A7.
   - **Data Inputs**:
     - IC2 Q0–Q3 to IC11 inputs B0–B3; tie IC12 B4–B7 to GND.
   - **Multiplexer Outputs**:
     - IC11–IC12 Y0–Y7 to **IC13 (27C16)** A0–A7 (Pins 2, 3, 4, 5, 6, 7, 8, 9).
   - **ROM**:
     - IC13 D0–D7 (Pins 11, 13, 15–20) to **IC7–IC10 (74LS47)** BCD inputs (A–D, Pins 7, 1, 2, 6).
     - Tie IC13 A8–A10 (Pins 10, 21, 23), CE (Pin 22), OE (Pin 20) to GND.
   - **7-Segment Displays**:
     - IC7–IC10 outputs a–g to DISP1–DISP4 segments a–g (optional 330 Ω resistors).
     - DISP1–DISP4 common anodes to 5V.
     - Tie IC7–IC10 LT (Pin 3), BI (Pin 4), RBI (Pin 5) to 5V.

## Operation
- **Write**:
  - Set SW1–SW8 (e.g., 00000011 = 3), SW9–SW12 (e.g., 1011 = 11), SW13 to program (0).
  - Clock pulse: WE = Low, OE = High; 1011 written to RAM at address 3.
  - LED1–LED8 show 00000011, LED9–LED12 show 1011.
  - SW14 selects address (2010 in base-5) or data (30 in base-5) on DISP1–DISP4.
- **Read**:
  - Set SW1–SW8 (e.g., 00000011), SW13 to read (1).
  - Clock pulse: WE = High, OE = Low; RAM outputs 1011 to IC2.
  - LED1–LED8 show 00000011, LED9–LED12 show 1011.
  - SW14 selects address (2010 in base-5) or data (30 in base-5) on DISP1–DISP4.

## ROM Contents for Base-5 Conversion
The **27C16 ROM** (2Kx8) converts 8-bit binary inputs (0–255) to base-5 digits for the four 7-segment displays. Data (0–15) requires two digits (00–30); addresses (0–255) require four digits (0000–2010).

### Conversion Logic
- **Data (0–15)**: Maps to base-5 00–30 (e.g., 15 = 3×5¹ + 0 = 30).
- **Address (0–255)**: Maps to base-5 0000–2010 (e.g., 255 = 2×5³ + 0×5² + 1×5¹ + 0 = 2010).
- Each ROM address outputs two BCD digits (8 bits: D7–D4 = digit 1, D3–D0 = digit 0).
- Use two ROM accesses per address input:
  - Address N (0x00–0xFF): Outputs digits d₁, d₀.
  - Address N+256 (0x100–0x1FF): Outputs digits d₃, d₂.

### ROM Table (Partial)
| ROM Address (Hex) | Input (Binary) | Base-5 Value | Output (BCD, D7–D0) | Display Digits |
|-------------------|----------------|--------------|---------------------|---------------|
| 00                | 00000000 (0)   | 00           | 0000 0000           | 0 0           |
| 01                | 00000001 (1)   | 01           | 0000 0001           | 0 1           |
| 02                | 00000010 (2)   | 02           | 0000 0010           | 0 2           |
| ...               | ...            | ...          | ...                 | ...           |
| 0F                | 00001111 (15)  | 30           | 0011 0000           | 3 0           |
| 10                | 00010000 (16)  | 31           | 0011 0001           | 3 1           |
| ...               | ...            | ...          | ...                 | ...           |
| FF                | 11111111 (255) | 2010         | 0010 0000           | 2 0*          |
| 100               | 00000000 (0)   | 00           | 0000 0000           | 0 0           |
| ...               | ...            | ...          | ...                 | ...           |
| 1FF               | 11111111 (255) | 2010         | 0001 0000           | 1 0*          |

*Note*: For addresses, ROM[0xFF] gives digits 1–2 (20), ROM[0x1FF] gives digits 3–4 (10). Toggle A8 (IC13 Pin 10) via control logic or a switch to select digit pairs.

### Programming the ROM
- **Size**: Use 512 bytes (0x00–0x1FF) of the 27C16’s 2048 bytes.
- **Algorithm**:
  - For input N (0–255):
    - Compute base-5: N = d₃×5³ + d₂×5² + d₁×5¹ + d₀×5⁰ (d₀–d₃ = 0–4).
    - Store d₁, d₀ as BCD in ROM[N].
    - Store d₃, d₂ as BCD in ROM[N+256].
  - Example: N = 255 → 2010 (base-5):
    - d₃ = 2, d₂ = 0, d₁ = 1, d₀ = 0.
    - ROM[0xFF] = 0010 0000 (2 0).
    - ROM[0x1FF] = 0001 0000 (1 0).
- **Tool**: Use a programmer (e.g., TL866II) to burn the ROM. Generate the 512-byte binary file with a script (e.g., Python):

```python
rom = bytearray(512)
for n in range(256):
    d3 = n // 125  # 5^3
    n %= 125
    d2 = n // 25   # 5^2
    n %= 25
    d1 = n // 5    # 5^1
    d0 = n % 5     # 5^0
    rom[n] = (d1 << 4) | d0
    rom[n + 256] = (d3 << 4) | d2
with open("rom.bin", "wb") as f:
    f.write(rom)