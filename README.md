# COA_Vikranth
Lab Experiments

#Table of Contents

1. [Universal Gates – Logic Gates Using NAND & NOR]
2. [Half Adder, Full Adder & 4-bit Adder]
3. [8×1 Multiplexer & 8×3 Encoder]
4. [4-bit Ripple Carry Adder & Propagation Delay]
5. [GNU Debugger (GDB) – Program Flow Analysis]
6. [Addressing Modes in C using GDB]
7. [4-bit Common Bus System using MUX & Registers]

## Experiment 1: Universal Gates – Logic Gates Using NAND & NOR

**Date:** Pre-lab (foundational experiment)

### Objective
To implement all fundamental logic gates (OR, AND, NOT, XOR, XNOR) using:
1. Only NAND gates
2. Only NOR gates

### Theory
NAND and NOR are called **universal gates** because any Boolean function — and therefore any digital circuit — can be built entirely from just one of these gate types. This property makes them crucial in real-world chip manufacturing, as fabricating a single gate type reduces cost and complexity.

### Implementations

#### Using NAND Gates Only

| Gate | NAND Implementation |
|------|----------------------|
| NOT A | `A NAND A` |
| A AND B | `(A NAND B) NAND (A NAND B)` — i.e., NOT of NAND |
| A OR B | `(A NAND A) NAND (B NAND B)` — i.e., NAND of NOTs |
| A XOR B | Use 4 NAND gates: `((A NAND B) NAND A) NAND ((A NAND B) NAND B)` |
| A XNOR B | NOT of XOR (add one more NAND as inverter) |

#### Using NOR Gates Only

| Gate | NOR Implementation |
|------|---------------------|
| NOT A | `A NOR A` |
| A OR B | `(A NOR B) NOR (A NOR B)` — i.e., NOT of NOR |
| A AND B | `(A NOR A) NOR (B NOR B)` — i.e., NOR of NOTs |
| A XNOR B | Use 4 NOR gates: `((A NOR B) NOR A) NOR ((A NOR B) NOR B)` |
| A XOR B | NOT of XNOR (add one more NOR as inverter) |

### Tools Used
- Logisim

---

## Experiment 2: Half Adder, Full Adder & 4-bit Adder

**Date:** 19-01-26

### Objective
To design and verify a Half Adder, Full Adder, and a 4-bit Adder on Logisim.

### Theory

**Half Adder** adds two single bits (A and B) and produces:
- **Sum** = A XOR B
- **Carry** = A AND B

It cannot account for a carry from a previous stage.

**Full Adder** adds three bits (A, B, and carry-in Cin) and produces:
- **Sum** = A XOR B XOR Cin
- **Carry-out** = (A AND B) OR (Cin AND (A XOR B))

**4-bit Adder** chains four Full Adders in series. The carry-out of each stage feeds into the carry-in of the next. This is called a **ripple carry adder**.

### Truth Tables

**Half Adder:**

| A | B | Sum | Carry |
|---|---|-----|-------|
| 0 | 0 |  0  |   0   |
| 0 | 1 |  1  |   0   |
| 1 | 0 |  1  |   0   |
| 1 | 1 |  0  |   1   |

**Full Adder:**

| A | B | Cin | Sum | Cout |
|---|---|-----|-----|------|
| 0 | 0 |  0  |  0  |  0   |
| 0 | 0 |  1  |  1  |  0   |
| 0 | 1 |  0  |  1  |  0   |
| 0 | 1 |  1  |  0  |  1   |
| 1 | 0 |  0  |  1  |  0   |
| 1 | 0 |  1  |  0  |  1   |
| 1 | 1 |  0  |  0  |  1   |
| 1 | 1 |  1  |  1  |  1   |

### Tools Used
- Logisim

---

## Experiment 3: 8×1 Multiplexer & 8×3 Encoder

**Date:** 02-02-26

### Objective
To design and verify an 8×1 Multiplexer and an 8×3 Encoder using basic logic gates on Logisim.

### Theory

**8×1 Multiplexer (MUX)**
A multiplexer is a combinational circuit that selects one of many input lines and forwards it to a single output line, based on **select lines**. An 8×1 MUX has:
- 8 data inputs (D0–D7)
- 3 select lines (S0, S1, S2)
- 1 output (Y)

The output Y = the data input selected by the binary value on the select lines.

**Boolean Expression:**
```
Y = D0·S2'·S1'·S0' + D1·S2'·S1'·S0 + D2·S2'·S1·S0' + D3·S2'·S1·S0
  + D4·S2·S1'·S0' + D5·S2·S1'·S0 + D6·S2·S1·S0' + D7·S2·S1·S0
```

**8×3 Encoder**
An encoder converts one of many active input signals into a binary code. An 8×3 (octal-to-binary) encoder has:
- 8 input lines (I0–I7), where exactly one is HIGH at a time
- 3 output lines (A2, A1, A0) representing the binary index of the active input

**Boolean Expressions:**
```
A2 = I4 + I5 + I6 + I7
A1 = I2 + I3 + I6 + I7
A0 = I1 + I3 + I5 + I7
```

### Tools Used
- Logisim

---

## Experiment 4: 4-bit Ripple Carry Adder & Propagation Delay

**Date:** 09-02-26

### Objective
To design and verify a 4-bit Ripple Carry Adder and to study the propagation delay on Logisim.

### Theory

A **4-bit Ripple Carry Adder (RCA)** is formed by cascading four Full Adders. It is called a "ripple" adder because the carry bit "ripples" from the least significant bit (LSB) to the most significant bit (MSB) — each stage must wait for the previous stage's carry-out before it can compute its own sum.

**Propagation Delay**
Since each full adder introduces a gate delay, and carries must propagate serially through all stages, the total propagation delay for an n-bit RCA is approximately:

```
T_total = n × T_FA
```

where `T_FA` is the delay of one full adder. For a 4-bit adder, the worst case occurs when a carry must ripple all the way from bit 0 to bit 3 (e.g., adding 0111 + 0001).

This delay grows linearly with bit width, which is why faster architectures like **Carry Look-ahead Adders (CLA)** are preferred for wider operands.

### Observations in Logisim
- Simulated propagation delay is measured by observing the time for the output to stabilize after changing inputs.
- Logisim's simulation clock can be used to step through gate-level delays.

### Tools Used
- Logisim

---

## Experiment 5: GNU Debugger (GDB) – Program Flow Analysis

**Date:** 16-03-26

### Objective
To study the GNU Debugger (GDB) and to analyze program flow.

### Theory

**GDB (GNU Debugger)** is a powerful debugging tool for programs written in C, C++, and other languages. It allows developers to:
- Pause execution at specific points (**breakpoints**)
- Step through code line by line
- Inspect the values of variables and registers
- Examine the call stack

### Key GDB Commands

| Command | Description |
|---------|-------------|
| `gcc -g program.c -o program` | Compile with debug symbols |
| `gdb ./program` | Start GDB with the program |
| `break main` | Set a breakpoint at `main` |
| `run` | Start program execution |
| `next` (or `n`) | Execute the next line (step over) |
| `step` (or `s`) | Step into a function call |
| `print <var>` | Print value of a variable |
| `info registers` | Display CPU register values |
| `backtrace` (or `bt`) | Show the current call stack |
| `continue` (or `c`) | Continue execution until next breakpoint |
| `quit` | Exit GDB |

### Program Flow Analysis
By stepping through a program, one can observe:
- How control passes between functions
- How loops iterate and terminate
- How conditional branches evaluate
- How the stack grows and shrinks with function calls

### Tools Used
- GDB (GNU Debugger)
- GCC Compiler
- Linux Terminal

---

## Experiment 6: Addressing Modes in C using GDB

**Date:** 06-04-26

### Objective
To study different addressing modes of instructions by analyzing the execution of a C program using the GNU Debugger.

### Theory

**Addressing modes** describe the methods a processor uses to access operands for its instructions. By compiling a C program and inspecting the generated assembly in GDB, each mode can be observed directly.

### Common Addressing Modes

| Mode | Description | C / Assembly Example |
|------|-------------|----------------------|
| **Immediate** | Operand is a constant value in the instruction | `int x = 5;` → `MOV EAX, 5` |
| **Register** | Operand is stored in a CPU register | `a + b` → `ADD EAX, EBX` |
| **Direct (Absolute)** | Operand's memory address is embedded in instruction | Global variable access |
| **Indirect** | Address is held in a register/pointer | `*ptr` → `MOV EAX, [EBX]` |
| **Base + Offset** | Address = base register + constant offset | Array/struct member access |
| **Indexed** | Address = base + index register (for arrays) | `arr[i]` → `MOV EAX, [EBX + ECX*4]` |
| **Stack (Implicit)** | Operand is pushed/popped from the stack | Function call arguments |

### GDB Commands for This Experiment

```bash
gcc -g -O0 program.c -o program   # -O0 disables optimizations for clear mapping
gdb ./program
(gdb) disassemble main             # View assembly of main function
(gdb) info registers               # Check register contents
(gdb) x/10x $esp                   # Examine stack memory
(gdb) stepi                        # Step one assembly instruction at a time
```

### Observations
Using `disassemble` and `stepi`, the assembly instructions generated for different C constructs reveal exactly which addressing mode the CPU uses for each operation.

### Tools Used
- GDB (GNU Debugger)
- GCC Compiler
- Linux Terminal

---

## Experiment 7: 4-bit Common Bus System using MUX & Registers

**Date:** 13-04-26

### Objective
To design and implement a 4-bit common bus system using multiplexers and registers on Logisim.

### Theory

A **common bus system** is a shared communication pathway through which data is transferred between multiple registers. Only one register can place data on the bus at a time; this is controlled by a **multiplexer** acting as a selector.

### System Architecture

The system consists of:
- **n registers** (each 4 bits wide), e.g., R0, R1, R2, R3
- **A 4×1 MUX per bit line** (since the bus is 4 bits wide, four 4×1 MUXes are used)
- **2 select lines (S1, S0)** to choose which register drives the bus

```
         S1 S0
          |  |
R0 ──►  [MUX] ──► BUS (4-bit)
R1 ──►  [MUX]
R2 ──►  [MUX]
R3 ──►  [MUX]
```

**How it works:**
- The select lines S1 and S0 determine which register's output is connected to the bus.
- The selected register's 4-bit value appears on the bus.
- Other registers (destinations) can read from the bus and load the value using a load enable signal.

**Select Line Truth Table:**

| S1 | S0 | Register on Bus |
|----|-----|-----------------|
|  0 |  0  | R0              |
|  0 |  1  | R1              |
|  1 |  0  | R2              |
|  1 |  1  | R3              |

### Significance
The common bus architecture is fundamental in CPU design — it is the basis of how the **ALU, registers, and memory** communicate internally within a processor's datapath.

### Tools Used
- Logisim

---

## 🛠️ Tools & Software

- **[Logisim](http://www.cburch.com/logisim/)** — Digital circuit simulator used for all hardware design experiments
- **GCC** — GNU C Compiler (`sudo apt install gcc`)
- **GDB** — GNU Debugger (`sudo apt install gdb`)

## 📁 Repository Structure

```
├── Experiment-1_Universal-Gates/
│   └── universal_gates.circ
├── Experiment-2_Adders/
│   └── adders.circ
├── Experiment-3_MUX-Encoder/
│   └── mux_encoder.circ
├── Experiment-4_Ripple-Carry-Adder/
│   └── ripple_carry_adder.circ
├── Experiment-5_GDB-Program-Flow/
│   └── program.c
├── Experiment-6_Addressing-Modes/
│   └── addressing_modes.c
├── Experiment-7_Common-Bus-System/
│   └── common_bus.circ
└── README.md
