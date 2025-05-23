# Sample Circuit for 4-Bit Logic System to Access RAM

This document describes a sample circuit for a 4-bit logic system, without a microprocessor, to interface with a small RAM chip (e.g., 2114 SRAM, 4-bit wide, 16-word). The circuit performs basic read and write operations using discrete logic components.

## Circuit Overview
The circuit uses a 4-bit counter to generate addresses, a 4-bit register for data input/output, and logic gates for control signals. It operates synchronously with a clock for proper timing.

## Components
1. **RAM Chip**:
   - **Example**: 2114 SRAM (1024x4-bit, using only 16 addresses).
   - **Specs**: 4-bit data bus, 10-bit address bus (using 4 bits), control signals (CE, WE, OE).
2. **4-Bit Binary Counter**:
   - **Example**: 74LS161 (synchronous 4-bit binary counter).
   - **Purpose**: Generates 4-bit addresses (A0–A3) for 16 memory locations.
3. **4-Bit Data Register**:
   - **Example**: 74LS173 (4-bit D-type register).
   - **Purpose**: Holds data for writing to RAM or stores data read from RAM.
4. **Clock Source**:
   - **Example**: 555 timer or crystal oscillator (e.g., 1 MHz).
   - **Purpose**: Synchronizes the counter and register.
5. **Control Logic**:
   - **Components**: 74LS00 (quad NAND gates), 74LS08 (quad AND gates).
   - **Purpose**: Generates control signals (CE, WE, OE) based on read/write selector.
6. **Read/Write Selector**:
   - **Example**: Manual switch or 74LS74 D flip-flop.
   - **Purpose**: Selects read or write operation.
7. **Input Switches**:
   - Four SPST switches for 4-bit data input.
   - **Purpose**: Sets data to write to RAM.
8. **Output Display**:
   - **Example**: Four LEDs or 7-segment display with 74LS47 decoder.
   - **Purpose**: Displays 4-bit data read from RAM.
9. **Pull-up/down Resistors**:
   - **Example**: 10kΩ resistors.
   - **Purpose**: Stabilizes inputs and prevents floating signals.
10. **Power Supply**:
    - 5V DC for TTL logic chips.

## Circuit Connections
1. **RAM (2114 SRAM)**:
   - **Address Lines (A0–A3)**: Connect to 74LS161 counter outputs (Q0–Q3). Ground unused address lines (A4–A9).
   - **Data Lines (D0–D3)**: Connect bidirectionally to 74LS173 register outputs (Q0–Q3) for writing and inputs (D0–D3) for reading.
   - **Control Lines**:
     - **CE (Chip Enable)**: Tie to ground (active low) or control logic output.
     - **WE (Write Enable)**: Connect to control logic (active low for write).
     - **OE (Output Enable)**: Connect to control logic (active low for read).
   - **Power**: Vcc to 5V, GND to ground.

2. **4-Bit Counter (74LS161)**:
   - **Clock Input**: Connect to clock source (e.g., 555 timer).
   - **Outputs (Q0–Q3)**: Connect to RAM address lines (A0–A3).
   - **Enable Inputs (ENP, ENT)**: Tie to Vcc for continuous counting.
   - **Clear**: Tie to Vcc or reset button (to ground).
   - **Power**: Vcc to 5V, GND to ground.

3. **4-Bit Data Register (74LS173)**:
   - **Data Inputs (D0–D3)**: Connect to switches (write) or RAM data lines (read).
   - **Data Outputs (Q0–Q3)**: Connect to RAM data lines (write) and LEDs/display (read).
   - **Clock Input**: Connect to clock source.
   - **Output Enable (OE1, OE2)**: Tie to ground or control logic.
   - **Power**: Vcc to 5V, GND to ground.

4. **Control Logic**:
   - Use a switch or 74LS74 flip-flop for read/write signal (R/W).
   - **Write (R/W = 0)**:
     - WE = (R/W NAND Clock) via 74LS00.
     - OE = High (disabled) via inverter (e.g., 74LS04).
     - CE = Low (active).
   - **Read (R/W = 1)**:
     - WE = High (disabled).
     - OE = (R/W AND Clock) via 74LS08.
     - CE = Low (active).
   - **Note**: CE can be tied low for simplicity.

5. **Clock Source (555 Timer)**:
   - Configure in astable mode (~1 Hz to 1 MHz, adjustable).
   - Output connects to counter and register clock inputs.

6. **Input/Output**:
   - **Input**: Four switches to data register inputs (D0–D3) via 10kΩ pull-down resistors.
   - **Output**: Four LEDs (via 330Ω resistors) or 7-segment display with 74LS47.

## Operation
1. **Initialization**:
   - Power on (5V).
   - Reset counter (74LS161) to address 0 if needed.

2. **Write Operation**:
   - Set R/W switch to write (R/W = 0).
   - Set switches to 4-bit data (e.g., 1011).
   - On clock pulse:
     - Counter outputs address (e.g., 0000).
     - Register loads switch data (1011).
     - Control logic: CE = Low, WE = Low, OE = High.
     - RAM stores 1011 at address 0000.
     - Counter increments to next address.

3. **Read Operation**:
   - Set R/W switch to read (R/W = 1).
   - On clock pulse:
     - Counter outputs address (e.g., 0000).
     - Control logic: CE = Low, WE = High, OE = Low.
     - RAM outputs data (e.g., 1011) to data lines.
     - Register captures data and displays on LEDs (e.g., ON-OFF-ON-ON).
     - Counter increments to next address.

4. **Continuous Operation**:
   - Clock cycles counter through addresses (0000 to 1111).
   - R/W switch determines read or write.

## Timing Considerations
- **Clock Speed**: Match RAM access time (e.g., 200 ns for 2114). Use slow clock (e.g., 1 kHz) for debugging.
- **Setup/Hold Times**: 74LS161/74LS173 ensure stable signals.
- **Control Timing**: Synchronize WE/OE with clock to avoid glitches.

## Example Signal Flow
### Write at Address 0000
- Clock pulse triggers.
- Counter outputs 0000.
- Switches set to 1011, loaded into register.
- R/W = 0: WE = Low, OE = High, CE = Low.
- RAM stores 1011 at 0000.
- Counter increments to 0001.

### Read at Address 0000
- Clock pulse triggers.
- Counter outputs 0000.
- R/W = 1: WE = High, OE = Low, CE = Low.
- RAM outputs 1011.
- Register captures 1011, displays on LEDs.
- Counter increments to 0001.

## Notes
- **Scalability**: Extend address bus (e.g., A4–A9) for full 2114 RAM access (1024 locations).
- **Simplifications**: Tie CE low; use manual clock (push-button) for step-by-step operation.
- **Debugging**: Use logic probe/oscilloscope to verify signals.
- **Alternatives**: Use CMOS chips (e.g., 74HC161, 74HC173) for TTL compatibility.

This circuit is a minimal design for educational purposes or simple applications like data logging. For a schematic or further details, please specify!