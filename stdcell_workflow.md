# Standard Cell Design & Characterization (GPDK180)

## Goal
We designed three fundamental building blocks ("standard cells") for digital chips:
1. **Inverter** (NOT gate) - Flips input logic (1→0, 0→1)
2. **NAND gate** - Outputs 0 only when ALL inputs are 1
3. **NOR gate** - Outputs 0 when ANY input is 1

Then, we combined these to create a **half adder** circuit that can:
- Add two 1-bit numbers (`a` and `b`)
- Produce:
  - `s` (sum) = `a XOR b` (1 if inputs differ)
  - `cout` (carry) = `a AND b` (1 if both inputs are 1)

---

## Workflow Explained

### Schematic Design 

- schematic - a diagram showing how components connect (like a circuit recipe).

**What we did**:
1. Opened **Cadence Virtuoso** (industry-standard chip design software)
2. Used pre-made transistors from `analogLib` (like Lego pieces)
3. Connected them to build:
   - Inverter: 1 PMOS + 1 NMOS transistor
   - NAND: 2 PMOS + 2 NMOS transistors  
   - NOR: 2 PMOS + 2 NMOS transistors (different connections)
4. Added **pins** (input/output ports) and **labels** (names for wires)
5. Tested with `vbit` (simulated input signals) to verify truth tables

---

### Symbol Creation 

- symbol - a simplified visual representation of your cell (hides internal details).

**What we did**:
1. Created symbols for each gate (drag-and-drop blocks)
2. Built **testbenches** (test environments) to:
   - Apply input combinations (00, 01, 10, 11)
   - Verify outputs match expected logic

---

### Layout Design 
**What's layout?**  
The actual silicon mask design (where transistors/wires physically sit).

**Key Constraints**:
- **12-track height**: All cells same height for easy stacking
- **Layers**:
  - **PolySi**: Forms transistor gates
  - **Metal1 (M1)**: Horizontal wires
  - **Metal2 (M2)**: Vertical wires

**Verification**:
1. **DRC (Design Rule Check)**:  
   - Checks if layout follows manufacturing rules (e.g., min wire spacing)
   - Like checking a house blueprint meets safety codes

2. **LVS (Layout vs Schematic)**:  
   - Confirms layout matches schematic connections
   - Like verifying the built house matches the architect's plans

3. **Parasitic Extraction (PEX)**:  
   - Calculates real-world wire resistances/capacitances
   - Outputs `.pex` netlist (circuit with delay effects)

---

### Characterization 
**What we measured**:
1. **Timing**:
   - `tpLH`: Delay when output changes 0→1
   - `tpHL`: Delay when output changes 1→0
2. **Power**:
   - Static (leakage when idle)
   - Dynamic (power during switching)
3. **Noise Margins**: How much noise the gate can tolerate

**Tools Used**:
- **Cadence Liberate**: Automates measurements across voltages/temperatures
- Output files:
  - `.lib`: Timing/power data (used by synthesis tools)
  - `.lef`: Abstract physical info (size/pin locations)

---

### Half Adder Synthesis 
**Implementation**:
- **Sum (s)**: Built from 5 NAND gates (to make XOR)
- **Carry (cout)**: Directly from 1 NAND + inverter

**Verification**:
- Compared synthesized circuit behavior vs truth table:
  
| a | b | s (XOR) | cout (AND) |
|---|---|--------|-----------|
| 0 | 0 | 0      | 0         |
| 0 | 1 | 1      | 0         |
| 1 | 0 | 1      | 0         |
| 1 | 1 | 0      | 1         |

---

## Tools Summary
| Tool | Purpose | Beginner Analogy |
|------|---------|------------------|
| **Cadence Virtuoso** | Draw schematics/layouts | Photoshop for circuits |
| **Assura** | Run DRC/LVS/PEX | Building inspector |
| **Liberate** | Characterize timing/power | Stopwatch + energy meter |
| **Genus/Design Compiler** | Synthesize circuits | Lego instruction generator |

---
