# UNIT-3: CPU Control Unit Design and Computer Arithmetic

## Part 1: Control Unit Design

The Control Unit (CU) is the "nervous system" of the CPU. It is responsible for decoding instructions and generating the appropriate timing and control signals to all other units (ALU, Memory, I/O) to execute those instructions.

### 1. Hardwired Control Unit

In this approach, the control logic is implemented directly using fixed combinational circuits (logic gates, flip-flops, decoders). It is effectively a complex finite state machine.

- **Structure & Design:**
  
  - **Instruction Decoder:** A decoder (e.g., 3x8 or 4x16) that takes the Operation Code (Opcode) bits and activates one specific output line corresponding to the instruction (e.g., $D_0$ for AND, $D_1$ for ADD).
  
  - **Step Decoder/Counter:** A Sequence Counter (SC) counts the timing steps (T0, T1, T2...). A decoder converts this count into individual timing signals.
  
  - **Control Logic Gates:** A massive array of AND, OR, and NOT gates combines the Instruction signals ($D_x$), Timing signals ($T_y$), and Condition flags (Zero, Sign) to produce specific Control Outputs (e.g., "Read Memory", "Load AC").

- **Logic Formula:** The logic for a specific control signal, such as "Increment PC", is derived by summing all conditions under which that action occurs:
  
  $$Control Signal (C_{in}) = (T_1 \cdot I_{fetch}) + (T_3 \cdot I_{add}) + \dots$$

- **Advantages:**
  
  - **Speed:** It is the fastest possible control implementation because the delay is only dependent on the propagation time through the logic gates.
  
  - **Chip Area:** For small instruction sets (RISC), it uses less silicon area than a ROM.

- **Disadvantages:**
  
  - **Inflexibility:** The logic is "hardwired" into the silicon. If a design error is found or a new instruction is needed, the entire physical chip must be redesigned and manufactured.
  
  - **Complexity:** As the instruction set grows, the combinatorial logic becomes exponentially complex and difficult to design and debug.

### 2. Micro-Programmed Control Unit

In this approach, the control logic is not random gates but a sequence of "micro-instructions" stored in a special memory. The CPU essentially runs a smaller program inside itself to execute the user's program.

- **Structure:**
  
  - **Control Address Register (CAR):** Holds the address of the next micro-instruction to be fetched from Control Memory.
  
  - **Control Memory (ROM):** A read-only memory that stores the micro-program. Each word in this memory is a "Micro-instruction" or "Control Word".
  
  - **Control Buffer Register (CBR):** Holds the current micro-instruction. The bits of this register directly drive the control lines of the CPU.
  
  - **Sequencer:** Determines the next address for the CAR (next step, branch, or map from Opcode).

- **Operation:**
  
  1. The machine instruction Opcode is "mapped" to a starting address in the Control Memory.
  
  2. The CU fetches a micro-instruction.
  
  3. It executes the micro-instruction (which asserts control lines).
  
  4. It determines the next address and repeats until the instruction is complete.

- **Advantages:**
  
  - **Flexibility:** The instruction set can be modified or fixed simply by updating the firmware (burning a new ROM) without changing the hardware structure.
  
  - **Structured Design:** Allows for designing Complex Instruction Set Computers (CISC) systematically.

- **Disadvantages:**
  
  - **Speed:** It is inherently slower than hardwired control because the CU must access the Control Memory for every step, introducing memory access latency.

#### Comparison Table

| Feature            | Hardwired Control                      | Micro-Programmed Control          |
| ------------------ | -------------------------------------- | --------------------------------- |
| **Speed**          | **Fast** (Combinational logic)         | **Slow** (Memory access required) |
| **Flexibility**    | Rigid (Requires hardware redesign)     | Flexible (Update microcode/ROM)   |
| **Implementation** | Logic Gates, Flip-Flops, Decoders      | Control Memory (ROM), Sequencer   |
| **Design Effort**  | High (Complex wiring for large sets)   | Low (Software-like micro-coding)  |
| **Application**    | RISC Processors (Simple, fixed instr.) | CISC Processors, Mainframes       |

## Part 2: Case Study - Design of a Simple Hypothetical CPU

To understand the practical interaction of these components, we analyze a "Basic Computer" model.

### Architecture Components

1. **Accumulator (AC):** The primary processor register used for arithmetic and logic operations.

2. **Program Counter (PC):** Holds the memory address of the *next* instruction to be executed.

3. **Instruction Register (IR):** Stores the instruction currently being executed so the decoder can analyze the Opcode.

4. **Memory Address Register (MAR):** The only register connected directly to the address bus; specifies which memory location to access.

5. **Memory Data Register (MDR):** The buffer for the data bus; holds data read from or written to memory.

### Instruction Cycle

The CPU operates in a perpetual loop of three phases: **Fetch -> Decode -> Execute**.

#### 1. Fetch Cycle (Common to all instructions)

This phase retrieves the instruction from memory.

- **T0:** $AR \leftarrow PC$
  
  - Transfer the address from PC to AR (MAR). The PC tracks execution, but the AR talks to memory.

- **T1:** $IR \leftarrow M[AR]$, $PC \leftarrow PC + 1$
  
  - Read memory at address AR and place data into IR. Simultaneously, increment PC to point to the next instruction.

- **T2:** Decode Opcode in IR(12-15), $AR \leftarrow IR(0-11)$
  
  - The decoder activates the specific signal (e.g., $D_1$ for ADD). The lower 12 bits (address part) are moved to AR to prepare for operand fetching.

#### 2. Execute Cycle (Example: ADD Operation)

Instruction: `ADD X` (Add content of memory address X to AC).

- **T3:** $DR \leftarrow M[AR]$
  
  - The AR already holds the address of the operand (from T2). The CPU reads the data into the Data Register (DR).

- **T4:** $AC \leftarrow AC + DR$, $SC \leftarrow 0$
  
  - The ALU adds the value in DR to the value in AC. The result is stored back in AC. The Sequence Counter (SC) is cleared to 0 to restart the cycle for the next instruction.

## Part 3: Computer Arithmetic Algorithms

### 1. Addition and Subtraction

Modern computers almost exclusively use **2's Complement** representation for signed integers because it simplifies hardware: subtraction can be performed using an adder.

- **Logic:**
  
  - **Addition:** $A + B$ (Standard binary addition).
  
  - **Subtraction:** $A - B = A + (-B)$. In 2's complement, $-B$ is obtained by inverting all bits of B and adding 1.

- **Hardware Implementation:** A single Parallel Adder circuit is used. The $B$ inputs pass through XOR gates controlled by a Mode bit ($M$).
  
  - **If M=0 (Add):** $B \oplus 0 = B$. The adder receives $A$ and $B$. $C_{in}=0$.
  
  - **If M=1 (Sub):** $B \oplus 1 = B'$ (1's complement). The adder receives $A$ and $B'$. $C_{in}$ is set to 1 to complete the 2's complement ($B' + 1$).

- **Overflow Detection:** For signed numbers, overflow occurs if adding two positives yields a negative or vice-versa. Logic: $V = C_n \oplus C_{n-1}$ (XOR of carry-in and carry-out of the MSB).

### 2. Multiplication (Booth's Algorithm)

Standard multiplication (Shift-and-Add) is inefficient for negative numbers. **Booth's Algorithm** is a smart algorithm that handles signed numbers uniformly and improves speed by skipping over strings of 1s.

**Algorithm Logic:** It scans the multiplier ($Q$) bits and recodes them. A string of 1s (e.g., `00111100`) is treated as the difference between the power of 2 at the start and end of the string ($2^6 - 2^2$).
Check two bits: $Q_0$ (current LSB) and $Q_{-1}$ (previous LSB).

1. **00 or 11:** The bits are identical (inside a string of 0s or 1s). **Do nothing** but Arithmetic Shift Right (ASR).

2. **01:** End of a string of 1s. **Add Multiplicand (**$A \leftarrow A + M$**)**, then ASR.

3. **10:** Start of a string of 1s. **Subtract Multiplicand (**$A \leftarrow A - M$**)**, then ASR.

**Example: Multiply 7 (**$M$**) by 3 (**$Q$**) using 4-bit Booth's.**

- $M = 0111$ (7)

- $Q = 0011$ (3)

- $-M = 1001$ (2's complement of 7)

| Step        | Action         | A (Accumulator) | Q (Multiplier) | Q-1 | Comment                                  |
| ----------- | -------------- | --------------- | -------------- | --- | ---------------------------------------- |
| **Init**    |                | 0000            | 0011           | 0   | Load values                              |
| **Cycle 1** | **10** (Sub M) | 1001            | 0011           | 0   | Start of 1s string ($2^0$)               |
|             | ASR            | 1100            | 1001           | 1   | Arithmetic Shift preserves sign          |
| **Cycle 2** | **11** (No Op) | 1100            | 1001           | 1   | Middle of 1s string                      |
|             | ASR            | 1110            | 0100           | 1   |                                          |
| **Cycle 3** | **01** (Add M) | 0101            | 0100           | 1   | End of 1s string ($2^2$)                 |
|             | ASR            | 0010            | 1010           | 0   | Effectively $+M \cdot 2^2 - M \cdot 2^0$ |
| **Cycle 4** | **00** (No Op) | 0010            | 1010           | 0   | String of 0s                             |
|             | ASR            | **0001**        | **0101**       | 0   |                                          |

**Result:** Combined AQ is `00010101` (21).

### 3. Division (Restoring Algorithm)

Division is strictly sequential and cannot be parallelized easily. **Logic:** It mimics manual long division. We assume the quotient bit is 1, subtract the divisor, and check the result.

1. **Shift Left** ($A, Q$): Bring the next bit of the dividend into the computation window (A).

2. **Subtract Divisor** ($A \leftarrow A - M$).

3. **Check sign of A**:
   
   - If **Positive (**$A \ge 0$**)**: Our assumption was correct. The divisor "fits". Set Quotient bit $Q_0 = 1$.
   
   - If **Negative (**$A < 0$**)**: Our assumption was wrong. The divisor was too big. Set Quotient bit $Q_0 = 0$ and **Restore** the value of A by adding M back ($A \leftarrow A + M$).

**Example: 11 / 3** (Using 4-bit unsigned)
Dividend (Q) = 1011 (11), Divisor (M) = 0011 (3), A = 0000.

| Step        | Action      | A         | Q    | Explanation                  |
| ----------- | ----------- | --------- | ---- | ---------------------------- |
| **Init**    |             | 0000      | 1011 |                              |
| **Cycle 1** | Shift Left  | 0001      | 011_ | Shift in MSB of Q            |
|             | Sub M       | 1110 (-2) | 011_ | A is Negative                |
|             | **Restore** | 0001      | 0110 | Add M back, set Q0=0         |
| **Cycle 2** | Shift Left  | 0010      | 110_ |                              |
|             | Sub M       | 1111 (-1) | 110_ | A is Negative                |
|             | **Restore** | 0010      | 1100 | Add M back, set Q0=0         |
| **Cycle 3** | Shift Left  | 0101      | 100_ |                              |
|             | Sub M       | 0010 (+2) | 100_ | A is Positive! Divisor fits. |
|             | **Set Bit** | 0010      | 1001 | Keep result, set Q0=1        |
| **Cycle 4** | Shift Left  | 0100      | 001_ |                              |
|             | Sub M       | 0001 (+1) | 001_ | A is Positive!               |
|             | **Set Bit** | 0001      | 0011 | Keep result, set Q0=1        |

**Result:** Q=`0011` (3), A=`0001` (Rem=1).

## Part 4: Input/Output Organization

### 1. I/O Addressing

The CPU must communicate with many peripherals. The addressing method determines how the CPU "talks" to them.

- **Memory Mapped I/O:**
  
  - I/O devices share the same address bus and space as RAM.
  
  - **Implication:** If you have 64KB address space and the video buffer uses 16KB, you only have 48KB left for programs.
  
  - **Programming:** The programmer uses standard `MOV`, `LOAD`, `STORE` instructions to manipulate hardware registers.

- **Isolated I/O (I/O Mapped):**
  
  - The CPU has distinct `Input` and `Output` control lines.
  
  - **Implication:** Memory Address `0x100` refers to RAM, while I/O Address `0x100` refers to a totally different device (e.g., a keyboard port).
  
  - **Programming:** Requires specific assembly instructions like `IN AL, Port` or `OUT Port, AL`.

### 2. Synchronization & Interfacing

Peripherals (keyboards, printers) are vastly slower than CPUs. Direct communication would crash the system or lose data.

- **Interface Units:** Hardware bridges (like UARTs) that convert signal formats (e.g., Serial to Parallel) and signal levels.

- **Handshaking (2-Wire or 3-Wire):** Instead of guessing when data is ready (Strobe), handshaking guarantees safe transfer.
  
  1. Source asserts `Request` (I have data).
  
  2. Destination asserts `Acknowledge` (I see your data, I am reading it).
  
  3. Source drops `Request` (Okay, I'm done).

### 3. Modes of Transfer

#### A. Programmed I/O (Polling)

The CPU stays in a tight software loop, constantly checking a status flag (Busy/Ready bit) from the interface.

- **Logic:** `while (device.busy) { /* wait */ } read_data();`

- **Pros:** Very simple hardware; no interrupts required.

- **Cons:** Extremely inefficient. The CPU cannot do any computational work while waiting for slow I/O (Busy Waiting), wasting millions of cycles.

#### B. Interrupt-Initiated I/O

The hardware solves the polling problem. The CPU works on other tasks; the device physically toggles a CPU pin when ready.

- **The Interrupt Cycle:**
  
  1. **Request:** Device asserts IRQ.
  
  2. **Finish:** CPU completes the *current* instruction.
  
  3. **Context Save:** CPU saves the **PC** (return address) and often the **PSW** (Program Status Word/Flags) onto the Stack.
  
  4. **Vectoring:** CPU fetches the address of the specific **ISR** (Interrupt Service Routine) from a vector table.
  
  5. **Service:** CPU executes the ISR code to handle the data.
  
  6. **Restore:** CPU executes `IRET` (Interrupt Return), popping PC/PSW from the stack to resume the original task exactly where it left off.

- **Priority Logic (Daisy Chaining):** If multiple devices interrupt at once, a hardware chain grants the bus to the device electrically closest to the CPU first.

#### C. Direct Memory Access (DMA)

For high-speed devices (Disk Drives, Network Cards), even Interrupts are too slow because the data must pass through the CPU registers (Disk -> CPU -> RAM). DMA bypasses the CPU.

- **Bus Master Concept:** Normally, the CPU is the "Master" of the memory bus. During DMA, the CPU relinquishes control, and the DMAC becomes the Bus Master.

- **Operation Steps:**
  
  1. **Initialization:** CPU programs the DMAC with: Start Address, Word Count, and Control Mode (Read/Write).
  
  2. **Bus Request (BR):** DMAC asks CPU for the bus.
  
  3. **Bus Grant (BG):** CPU finishes current bus cycle, disconnects from the bus (high-impedance state), and asserts BG.
  
  4. **Transfer:** DMAC drives the address and data bus directly to move a block of data to/from RAM.
  
  5. **Completion:** When the Word Count reaches zero, DMAC releases the bus and sends an Interrupt to the CPU to say "Job Done".

- **Types:**
  
  - **Burst Mode:** Fast but blocks CPU for long duration.
  
  - **Cycle Stealing:** Interleaves DMA transfers with CPU cycles; slower transfer but keeps CPU responsive.

#### D. I/O Processors (IOP)

In mainframes and high-end servers, the DMA concept is expanded into a full computer.

- The IOP is a specialized CPU with its own instruction set optimized for I/O (channels).

- The main CPU effectively outsources all I/O operations. It constructs a "Channel Program" in memory and tells the IOP "Execute this".

- The IOP handles all handshaking, error retries, and data formatting, only interrupting the main CPU when the entire complex task is finished.
