# Unit 1: Computer Architecture Study Notes

## Basic Functional Blocks & Instruction Set Architecture

### 1. Basic Functional Blocks of a Computer

A computer system is defined by the interaction of its three main subsystems: the Central Processing Unit (CPU), the Memory Subsystem, and the Input/Output Subsystem.

**Conceptual Diagram:**

```
      +------------------+
      |   Input Device   |
      +--------+---------+
               | Data
               v
      +--------+---------+          +------------------+
      |       CPU        |<-------->|   Memory Unit    |
      | +--------------+ | Reads/   +------------------+
      | |      ALU     | | Writes
      | +--------------+ |
      | | Control Unit | |
      | +--------------+ |
      | |  Registers   | |
      | +--------------+ |
      +--------+---------+
               | Data
               v
      +--------+---------+
      |  Output Device   |
      +------------------+
```

#### The Control Unit (Focus Area)

The Control Unit (CU) is the "nervous system" of the computer. It does not execute program instructions itself; rather, it directs other parts of the system to do so.

**Control Unit Block Diagram:** This diagram illustrates how the Control Unit takes inputs (Clock, Opcode, Flags) and generates control signals.

```
       Inputs
   +------------------+
   |      Clock       |------+
   +------------------+      |
                             v
   +------------------+   +------------+      +-------------------+
   | IR (Opcode)      |-->|  Control   |----->| ALU Control Sigs  |
   +------------------+   |    Unit    |      +-------------------+
                          |   Logic /  |
   +------------------+   |   Decoder  |----->| Memory Read/Write |
   |      Flags       |-->|            |      +-------------------+
   +------------------+   +------------+
                                 |
                                 +----------->| Reg Select/Load   |
                                              +-------------------+
                                                  Outputs
```

**Primary Functions:**

- **Timing:** Generates clock signals to synchronize operations.

- **Decoding:** Interprets instructions fetched from memory.

- **Control Signals:** Issues electrical signals to the ALU, Registers, and Memory to initiate actions (e.g., "Read", "Write", "Add").

**Types of Control Units:**

| Feature            | Hardwired Control Unit                                          | Microprogrammed Control Unit                             |
| ------------------ | --------------------------------------------------------------- | -------------------------------------------------------- |
| **Implementation** | Built using combinatorial logic circuits (gates, flip-flops).   | Built using "Control Memory" (firmware/micro-code).      |
| **Speed**          | Very fast (direct hardware execution).                          | Slower (requires fetching micro-instructions).           |
| **Flexibility**    | Rigid. Changing the instruction set requires rewiring the chip. | Flexible. Changes can be made by updating the microcode. |
| **Complexity**     | Becomes complex with large instruction sets (CISC).             | Easier to design for complex instruction sets.           |
| **Application**    | RISC processors (Reduced Instruction Set).                      | CISC processors (Complex Instruction Set).               |

### 2. Instruction Set Architecture (ISA)

The ISA is the interface between the software and the hardware. It defines what the processor is capable of doing.

#### CPU Registers

Registers are small, high-speed storage locations inside the CPU.

1. **General Purpose Registers (GPR):** Used by programmers to hold data/operands (e.g., R0, R1, AX, BX).

2. **Special Purpose Registers (SPR):**
   
   - **PC (Program Counter):** Holds the address of the *next* instruction to be executed.
   
   - **IR (Instruction Register):** Holds the *current* instruction being executed.
   
   - **MAR (Memory Address Register):** Holds the address of the memory location to be accessed.
   
   - **MDR/MBR (Memory Data/Buffer Register):** Holds the data being read from or written to memory.

#### Instruction Execution Cycle

The process by which a computer executes a program.

**Cycle Flowchart:**

```
    +-------+      +--------+      +---------+      +-------+
    | Fetch |----->| Decode |----->| Execute |----->| Store |
    +-------+      +--------+      +---------+      +-------+
        ^                                               |
        |                                               |
        +-----------------------------------------------+
                        Next Instruction
```

1. **Fetch:** The CPU reads the instruction from memory (pointed to by PC) into the IR.

2. **Decode:** The Control Unit interprets the bits in the IR to determine the operation.

3. **Execute:** The Control Unit signals the necessary parts (ALU, etc.) to perform the operation.

4. **Store:** The result is written back to memory or a register.

**RTL (Register Transfer Language) Interpretation:** RTL is a notation used to describe the micro-operations. The following diagram visualizes the data movement during the **Fetch Phase**:

```
   Step 1: Address Transfer
   +----+       +-----+
   | PC | ----> | MAR |
   +----+       +-----+

   Step 2: Memory Access & PC Increment
   +-----+      +--------+      +-----+
   | MAR | ---> | Memory | ---> | MDR |
   +-----+      +--------+      +-----+
                    ^
                    |
              +------------+
              | PC = PC+1  |
              +------------+

   Step 3: Instruction Load
   +-----+      +----+
   | MDR | ---> | IR |
   +-----+      +----+
```

- *Textual Representation of Fetch Cycle:*
  
  1. `MAR ← PC` (Move Program Counter to Address Register)
  
  2. `MDR ← M[MAR]`, `PC ← PC + 1` (Read Memory into Data Register, Increment PC)
  
  3. `IR ← MDR` (Move data to Instruction Register)

### 3. Addressing Modes (Deep Dive)

Addressing modes specify the rule for interpreting or modifying the address field of the instruction before the operand is actually referenced.

**Key Concept: Effective Address (EA)** The **Effective Address** is the final, actual physical memory address where the operand is located. The goal of most addressing modes is to calculate the EA.

#### Common Addressing Modes

##### 1. Implied (Implicit) Mode

- **Definition:** The operand is specified implicitly in the definition of the instruction.

- **Mechanism:** No address field is needed.

- **Example:** `CLA` (Clear Accumulator). The operand is known to be the Accumulator.

- **EA Calculation:** N/A.

##### 2. Immediate Mode

- **Definition:** The operand is part of the instruction itself.

- **Mechanism:** The address field contains the value, not an address.

- **Example:** `MOV R1, #50` (Move value 50 into R1).

- **EA Calculation:** No memory reference for data.

- **Pros/Cons:** Fast (no memory fetch), but limited constant range.

##### 3. Register Mode

- **Definition:** The operand is stored in a CPU register.

- **Mechanism:** The instruction specifies the register ID.

- **Example:** `ADD R1, R2` (Add contents of R2 to R1).

- **EA Calculation:** EA = Register File.

- **Pros/Cons:** Very fast (no memory access), but limited number of registers.

##### 4. Register Indirect Mode

- **Definition:** The instruction specifies a register that *contains* the address of the operand in memory.

- **Mechanism:** The register acts as a pointer.

- **Example:** `MOV R1, (R2)` (Move data from memory address stored in R2 to R1).

- **EA Calculation:** `EA = Content of Register` or `EA = [R]`

- **Pros/Cons:** Large address space access, one fewer memory reference than indirect mode.

##### 5. Direct (Absolute) Mode

- **Definition:** The instruction contains the direct address of the operand in memory.

- **Mechanism:** The address field is the EA.

- **Example:** `LOAD R1, 1000` (Load data from memory location 1000).

- **EA Calculation:** `EA = Address Field (A)`

- **Pros/Cons:** Simple, but limits address space to the size of the instruction's address field.

##### 6. Indirect Mode

- **Definition:** The instruction contains a memory address, which in turn contains the address of the operand. (A pointer to a pointer).

- **Mechanism:** Two memory accesses are required to get the data.

- **Example:** `LOAD R1, @1000` (Go to 1000, read the address there, go to that address to get data).

- **EA Calculation:** `EA = M[Address Field]`

- **Pros/Cons:** Large address space, but slow (multiple memory accesses).

##### 7. Displacement Addressing Modes

These modes combine a register value and a constant to calculate the EA. *Formula:* `EA = [Register] + Offset`

- **A. Relative Addressing:**
  
  - Uses the **Program Counter (PC)**.
  
  - `EA = PC + Offset`.
  
  - Used for branching (`JUMP`) to nearby code limits (Locality of Reference).

- **B. Base Register Addressing:**
  
  - Uses a **Base Register**.
  
  - `EA = Base Register + Offset`.
  
  - Used for relocating programs in memory. The Base holds the starting address of the program segment.

- **C. Indexed Addressing:**
  
  - Uses an **Index Register**.
  
  - `EA = Index Register + Base Address`.
  
  - Used for arrays. The address field is the start of the array, the register holds the index `i`.

#### Summary Comparison Table

| Mode             | Operand Location | Effective Address (EA) | Speed     | Use Case                   |
| ---------------- | ---------------- | ---------------------- | --------- | -------------------------- |
| **Immediate**    | In Instruction   | N/A                    | Fastest   | Constants, Initialization  |
| **Register**     | In Register      | N/A                    | Very Fast | Temporary computations     |
| **Direct**       | In Memory        | `EA = A`               | Fast      | Global variables           |
| **Indirect**     | In Memory        | `EA = M[A]`            | Slow      | Pointers, Passing params   |
| **Reg Indirect** | In Memory        | `EA = [R]`             | Moderate  | Pointers, Array traversal  |
| **Relative**     | In Memory        | `EA = PC + Offset`     | Moderate  | Loops, Branching (If/Else) |
| **Indexed**      | In Memory        | `EA = IndexReg + A`    | Moderate  | Arrays (`A[i]`)            |

### 4. Instruction Sets

An Instruction Set is the collection of instructions that a specific CPU is designed to execute.

#### Instruction Formats

The layout of bits in an instruction. It typically includes an Opcode (what to do) and Operands (who to do it to).

1. **Three-Address Instruction:** `Opcode | Dest | Src1 | Src2`
   
   - `ADD R1, A, B` (R1 <- M[A] + M[B])
   
   - Short programs, but long instruction bits.

2. **Two-Address Instruction:** `Opcode | Dest/Src1 | Src2`
   
   - `ADD R1, B` (R1 <- R1 + M[B])
   
   - Most common in commercial computers.

3. **One-Address Instruction:** `Opcode | Operand`
   
   - `ADD B` (Accumulator <- Accumulator + M[B])
   
   - Relies on an implicit Accumulator register.

4. **Zero-Address Instruction:** `Opcode`
   
   - `ADD`
   
   - Uses a **Stack** architecture. Operands are popped from the stack, added, and result is pushed back.

#### Types of Instructions

1. **Data Transfer:** Move data between registers and memory (MOV, LOAD, STORE, PUSH, POP).

2. **Data Manipulation:**
   
   - *Arithmetic:* ADD, SUB, MUL, DIV.
   
   - *Logical:* AND, OR, NOT, XOR.
   
   - *Shift:* Shift Logical, Shift Arithmetic (Rotate).

3. **Program Control:** Change the flow of execution (JUMP, BRANCH, CALL, RETURN, HALT).
