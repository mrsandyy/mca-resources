# Unit-2: Computer System Architecture - In-Depth Study Notes

## 1. Logic Gates

Logic gates are the fundamental building blocks of any digital system. They are electronic circuits having one or more inputs and only one output. The relationship between the input and the output is based on a specific logic.

*Figure 1: Unified Symbols for Logic Gates (Buffer, NOT, AND, NAND, OR, NOR, XOR, XNOR)*

### A. Basic Gates

These are the simplest forms of logic.

- **AND Gate:** The output is High (1) only if **all** inputs are High. It performs logical multiplication.
  
  - *Symbolic Logic:* $Y = A \cdot B$

- **OR Gate:** The output is High (1) if **any** one of the inputs is High. It performs logical addition.
  
  - *Symbolic Logic:* $Y = A + B$

- **NOT Gate (Inverter):** The output is the complement of the input. If input is 1, output is 0.
  
  - *Symbolic Logic:* $Y = A'$ or $\bar{A}$

### B. Universal Gates

A Universal Gate is a gate that can be used to implement *any* Boolean function without using any other type of gate.

- **NAND Gate (Not-AND):** The output is Low (0) only if all inputs are High. It is the complement of the AND gate.

- **NOR Gate (Not-OR):** The output is High (1) only if all inputs are Low. It is the complement of the OR gate.

### C. Arithmetic/Special Gates

- **XOR Gate (Exclusive-OR):** The output is High (1) if the inputs are **different** (e.g., 0,1 or 1,0). It is used extensively in binary addition (Half Adders).
  
  - *Symbolic Logic:* $Y = A \oplus B = A'B + AB'$

- **XNOR Gate (Exclusive-NOR):** The output is High (1) if the inputs are the **same** (e.g., 0,0 or 1,1). It represents equality.

## 2. Map Simplification (Karnaugh Maps)

The Karnaugh Map (K-Map) is a graphical method used to minimize Boolean expressions without using Boolean algebra theorems. It reduces the number of logic gates required.

*Figure 2: Example of a 2-variable K-Map grouping*

### Concept & Definitions

- **The Grid:** A K-Map is a grid of cells. Each cell represents a binary combination of input variables.

- **Gray Code:** The rows and columns are arranged using Gray Code (00, 01, 11, 10) so that only one bit changes between adjacent cells. This physical adjacency allows for simplification.

- **Implicants:**
  
  - **Group/Pair:** A group of 2 adjacent 1s (eliminates 1 variable).
  
  - **Quad:** A group of 4 adjacent 1s (eliminates 2 variables).
  
  - **Octet:** A group of 8 adjacent 1s (eliminates 3 variables).

- **Don't Care Conditions (d or X):** Input combinations that will never occur or where the output state does not matter. These can be treated as either 0 or 1 to help form larger groups for better simplification.

## 3. Combinational Circuits

A combinational circuit is a circuit where the output at any instant depends **only** on the present input values. It has no memory element and no feedback loop.

### A. Adders

Circuits used to perform binary addition.

- **Half Adder:**
  
  - **Definition:** Adds two single binary digits ($A$ and $B$).
  
  - **Outputs:** Sum ($S$) and Carry ($C$).
  
  - **Logic:** $Sum = A \oplus B$ (XOR), $Carry = A \cdot B$ (AND).
  
  - *Limitation:* Cannot handle a carry coming from a previous lower significant bit.

*Figure 3: Half Adder Circuit*

- **Full Adder:**
  
  - **Definition:** Adds three single binary digits (Input A, Input B, and Carry-in $C_{in}$).
  
  - **Outputs:** Sum ($S$) and Carry-out ($C_{out}$).
  
  - **Logic:** Constructed using two Half Adders and an OR gate.

*Figure 4: Full Adder Circuit*

### B. Multiplexers (MUX)

- **Definition:** Also known as a "Data Selector." It has multiple input lines ($2^n$), $n$ selection lines, and a single output line.

- **Function:** The selection lines determine which specific input is connected to the output.

- **Logic:** A 4:1 Mux has 4 inputs ($I_0$ to $I_3$), 2 selection lines ($S_0, S_1$), and 1 Output ($Y$).

*Figure 5: 4:1 Multiplexer Internal Circuit*

### C. Decoders

- **Definition:** A circuit that converts binary information from $n$ input lines to a maximum of $2^n$ unique output lines.

- **Function:** It detects a specific bit pattern (code) at the input and activates the corresponding single output line.

- **Common Type:** 3-to-8 Line Decoder (Binary to Octal).

*Figure 6: 3-to-8 Line Decoder Symbol*

### D. Encoders

- **Definition:** The reverse operation of a decoder. It has $2^n$ (or fewer) input lines and $n$ output lines.

- **Function:** It generates a binary code corresponding to the active input line.

- **Priority Encoder:** A special type of encoder where if two inputs are active simultaneously, the output corresponds to the input with the highest priority.

## 4. Sequential Circuits

A sequential circuit is a circuit where the output depends on the **present inputs** AND the **past history** (stored state) of inputs. These circuits require memory.

### A. Flip-Flops (The Memory Element)

A Flip-Flop is a bistable multivibrator capable of storing 1 bit of data.

1. **SR Flip-Flop (Set-Reset):**
   
   - Has two inputs: S (Set) and R (Reset).
   
   - **Problem:** If $S=1$ and $R=1$, the output is undefined (Invalid state).

  *Figure 7: Clocked SR Flip-Flop*

2. **D Flip-Flop (Data/Delay):**
   
   - Has one input: D.
   
   - Eliminates the invalid state of the SR flip-flop.
   
   - **Function:** Output $Q$ simply follows input $D$ after the clock edge. Used for data storage.

  *Figure 8: D Flip-Flop Diagram*

3. **JK Flip-Flop:**
   
   - Modified SR flip-flop.
   
   - **Function:** If $J=1$ and $K=1$, the output **toggles** (complements the previous state). This fixes the SR invalid state.

  *Figure 9: JK Flip-Flop Symbol*

4. **T Flip-Flop (Toggle):**
   
   - Derived from JK (where $J=K=T$).
   
   - **Function:** Toggles the output state when $T=1$. Used in counters.

### B. Registers

A Register is a group of Flip-Flops arranged to store multiple bits of data.

- **Clocking:** All flip-flops in a register are usually triggered by a common clock signal.

#### Types of Data Movement (Shift Registers)

1. **SISO (Serial-In Serial-Out):** Data enters one bit at a time and exits one bit at a time. Slowest transfer.

2. **SIPO (Serial-In Parallel-Out):** Data enters serially but is available at all outputs simultaneously.

3. **PISO (Parallel-In Serial-Out):** Data is loaded simultaneously into all flip-flops and shifted out one by one.

4. **PIPO (Parallel-In Parallel-Out):** Data is loaded simultaneously and output simultaneously.

*Figure 10: SISO Shift Register Circuit*
