# Unit 4: Memory System and Performance Enhancement Techniques

## Part 1: Pipelining

### 1. Basic Concepts of Pipelining

Pipelining is an advanced implementation technique used to increase the CPU's instruction throughput—the number of instructions completed per unit of time. Rather than executing one instruction completely before starting the next, pipelining allows multiple instructions to overlap in execution.

It is best understood through the analogy of an automobile assembly line: while one car is having its engine installed, the car ahead of it is getting its chassis painted, and the car behind it is having its wheels attached. No single task happens faster, but the rate at which finished cars roll off the line increases dramatically.

**The Standard 5-Stage RISC Pipeline:** To strictly organize this overlap, the execution of a standard MIPS-style RISC instruction is broken down into 5 distinct stages. Each stage takes one clock cycle:

1. **IF (Instruction Fetch):** The processor uses the Program Counter (PC) to fetch the instruction from memory into the Instruction Register (IR). Simultaneously, the PC is incremented to point to the next sequential instruction ($PC \leftarrow PC + 4$).

2. **ID (Instruction Decode):** The control unit decodes the instruction to determine the operation. Concurrently, the values of the source registers are read from the Register File.

3. **EX (Execute):** The Arithmetic Logic Unit (ALU) performs the operation (e.g., add, subtract) or calculates the effective address for memory access.

4. **MEM (Memory Access):** For Load/Store instructions, the memory is accessed to read data (Load) or write data (Store). For arithmetic instructions, this stage is often idle.

5. **WB (Write Back):** The result of the operation (from the ALU) or the data fetched from memory is written back into the destination register in the Register File.

#### Visualizing the Pipeline

Imagine executing 3 instructions ($I_1, I_2, I_3$).

**Non-Pipelined (Sequential):** In a non-pipelined processor, the hardware remains idle between stages, or the entire datapath is dedicated to one instruction until completion.

```
Time --->
I1: [IF][ID][EX][MEM][WB]
I2:                      [IF][ID][EX][MEM][WB]
I3:                                           [IF][ID][EX][MEM][WB]
(15 cycles total for 3 instructions)
```

**Pipelined:** In a pipelined processor, as soon as $I_1$ moves to the ID stage, the hardware for the IF stage becomes free, allowing $I_2$ to enter immediately.

```
Time --->
I1: [IF][ID][EX][MEM][WB]
I2:     [IF][ID][EX][MEM][WB]
I3:         [IF][ID][EX][MEM][WB]
(7 cycles total - New instruction starts every clock cycle)
```

### 2. Throughput and Speedup

#### Terminology

- **Cycle Time (**$\tau$**):** The duration of a clock cycle. It is determined by the time required to complete the slowest stage in the pipeline (the bottleneck).

- **Throughput:** The rate at which instructions are completed. In an ideal pipeline, this is 1 instruction per cycle.

- **Latency:** The total time required to execute a single instruction from start to finish (5 cycles in our model).

#### Calculations

Let $k$ be the number of stages (segments) in the pipeline.
Let $n$ be the number of instructions to be executed.
Let $t_p$ be the clock cycle time of the pipeline.

**Time taken for Pipelined execution (**$T_p$**):** The first instruction requires $k$ cycles to fill the pipeline (latency). Once the pipeline is full, one instruction completes every subsequent cycle. Therefore, the remaining $(n-1)$ instructions take 1 cycle each.

$$T_p = (k + (n - 1)) \times t_p$$

**Time taken for Non-Pipelined execution (**$T_{np}$**):** Without pipelining, every instruction must wait for the previous one to finish completely.

$$T_{np} = n \times k \times t_p$$

**Speedup (**$S$**):** Speedup measures how much faster the pipelined version is compared to the non-pipelined version.

$$S = \frac{T_{np}}{T_p} = \frac{n \times k}{k + (n - 1)}$$

*Ideal Speedup:* As $n \to \infty$ (when executing millions of instructions), the term $(n-1)$ dominates $k$. The equation simplifies to $\frac{nk}{n}$, meaning the speedup approaches $k$ (the number of stages).

**Example Calculation:**

- **Problem:** A 4-stage pipeline executes 100 instructions. The cycle time is 10ns.

- **Solution:**
  
  - $k = 4$ stages, $n = 100$ instructions, $t_p = 10ns$.
  
  - Non-Pipelined Time: $100 \times 4 \times 10ns = 4000ns$.
  
  - Pipelined Time: The first instruction takes 4 cycles. The next 99 take 1 cycle each. Total cycles = $4 + 99 = 103$. Time = $103 \times 10ns = 1030ns$.
  
  - Speedup: $4000 / 1030 \approx 3.88$.
  
  - *Analysis:* The speedup is close to 4 (the ideal limit), but slightly lower due to the time taken to fill the pipeline initially.

### 3. Pipeline Hazards

Hazards are obstacles that prevent the next instruction in the stream from executing during its designated clock cycle. They force the pipeline to stall, reducing performance.

#### A. Structural Hazards (Hardware Conflicts)

These occur when hardware resources are insufficient to support all executing instructions simultaneously.

- **Scenario:** In the 5-stage pipeline, instruction $I_1$ is in the **MEM** stage (trying to read data from memory) while instruction $I_4$ is in the **IF** stage (trying to fetch an instruction from memory).

- **Conflict:** If the system uses a single unified memory (Von Neumann architecture) with only one read port, it cannot service both requests in the same clock cycle.

- **Solution:** Modern processors use the **Harvard Architecture** approach at the cache level, employing separate L1 Instruction Cache and L1 Data Cache to allow simultaneous access.

#### B. Data Hazards (Data Dependencies)

These occur when an instruction depends on the result of a previous instruction that has not yet completed. The pipeline exposes the latency of operations.

**Types:**

1. **RAW (Read After Write):** A true data dependency. An instruction tries to read a source before a previous instruction has written the new value.
   
   ```
   ADD R1, R2, R3  ; Writes result to R1 (Result available at end of stage 5/WB)
   SUB R4, R1, R5  ; Reads R1 (Needs R1 at start of stage 2/ID)
   ```
   
   *Without intervention, SUB reads the old, stale value of R1.*

2. **WAR (Write After Read):** Anti-dependency (rare in simple pipelines).

3. **WAW (Write After Write):** Output dependency (occurs in complex out-of-order processors).

**Solutions:**

1. **Stalling (Bubbles):** The hardware detects the hazard and injects "bubbles" (No-Operation or NOP instructions) into the pipeline. This pauses the dependent instruction until the data is ready, effectively lowering throughput.

2. **Operand Forwarding (Bypassing):** This is a hardware optimization. The result of the ADD instruction is actually available at the end of the **EX** stage. Special data paths forward this result directly to the ALU input of the SUB instruction, bypassing the need to wait for the WB stage.

#### C. Control Hazards (Branch Hazards)

These occur due to jump or branch instructions that change the flow of execution (the Program Counter).

- **Problem:** The pipeline fetches instructions $I+1$ and $I+2$ immediately after $I$. However, if $I$ is a branch instruction, the decision to jump is often not made until the MEM stage. By then, the pipeline has already fetched the wrong instructions.

- **Solutions:**
  
  1. **Pipeline Flush:** If the branch is taken, the instructions currently in the pipeline are discarded (flushed), and the correct address is fetched. This wastes cycles.
  
  2. **Branch Prediction:** The CPU "guesses" the outcome (e.g., assuming a loop branch is usually Taken). If the guess is right, zero penalty. If wrong, the pipeline must flush.
  
  3. **Delayed Branching:** The compiler reorders code to place a useful instruction (that needs to execute regardless of the branch outcome) immediately after the branch.

## Part 2: Memory Organization

### 1. Memory Interleaving

Main memory (DRAM) is significantly slower than the processor. To bridge this gap and increase bandwidth, memory is physically divided into independent modules called banks.

**Concept:** Instead of accessing one large block of memory sequentially, we arrange banks so they can be accessed in parallel.

- **Low-Order Interleaving:** Consecutive memory addresses are spread across different banks. For example, with 4 banks:
  
  - Address 0 $\rightarrow$ Bank 0
  
  - Address 1 $\rightarrow$ Bank 1
  
  - Address 2 $\rightarrow$ Bank 2
  
  - Address 3 $\rightarrow$ Bank 3

- **Benefit:** While Bank 0 is recovering from a read operation (recharging capacitors), the memory controller can initiate a read from Bank 1. If the CPU requests a block of sequential data (like an array), the effective access time is reduced by a factor of the number of banks ($N$).

### 2. Memory Hierarchy

Computer memory is organized in a pyramid structure to optimize the trade-off between cost, speed, and capacity. The goal is to provide the illusion of a memory that is as fast as the most expensive memory and as large as the cheapest technology.

1. **Registers:** (Top) Located inside the CPU core. Zero latency. Extremely limited capacity (e.g., 32 or 64 registers).

2. **Cache (L1, L2, L3):** Built using Static RAM (SRAM). Very fast but expensive. Holds the most frequently used data.

3. **Main Memory (RAM):** Built using Dynamic RAM (DRAM). Slower than SRAM but much denser and cheaper.

4. **Secondary Storage (SSD/HDD):** (Bottom) Non-volatile, vast capacity, but extremely slow (milliseconds vs nanoseconds).

**Principle of Locality:** Caches work because programs do not access memory randomly.

- **Temporal Locality:** If data is referenced, it will likely be referenced again soon. (e.g., loop counters, repeatedly called functions).

- **Spatial Locality:** If data is referenced, nearby addresses will likely be referenced soon. (e.g., traversing an array or executing sequential code instructions).

### 3. Cache Memory Basics

The Cache acts as a high-speed buffer. When the CPU requests data, it checks the cache first.

1. **Hit:** The data is found in the Cache. The CPU proceeds at full speed.

2. **Miss:** The data is not found. The CPU must stall while the data is fetched from the slower Main Memory, loaded into the Cache, and then delivered to the CPU.

**Performance Formula:**

$$AMAT = Hit\_Time + (Miss\_Rate \times Miss\_Penalty)$$

- **Hit Time:** Time to access the cache (usually 1-3 cycles).

- **Miss Rate:** The percentage of accesses that fail to find data in the cache ($1 - Hit\_Rate$).

- **Miss Penalty:** The time required to fetch the block from main memory (can be 100+ cycles).

- *Insight:* Even a small 1% miss rate can drastically ruin performance because the Miss Penalty is so high.

#### Cache Size vs. Block Size

- **Cache Size:** Total storage capacity (e.g., 4MB).

- **Block (Line) Size:** The minimum unit of data transfer (e.g., 64 bytes). When a single byte is requested, the entire block containing that byte is fetched.

- **Trade-off:** * **Larger Blocks:** Improve Spatial Locality (fetching neighbors) and reduce the overhead of tag storage.
  
  - **Downside:** Increases Miss Penalty (takes longer to transfer the block) and risks "Cache Pollution" (filling the cache with data that is never actually used).

### 4. Cache Mapping Functions (Crucial Topic)

The mapping function determines where a block from main memory can be placed in the cache. This is defined by how the hardware interprets the bits of the physical address.

**Assumptions for Examples:**

- Main Memory Size = 64 KB ($2^{16}$ bytes) $\rightarrow$ 16-bit Physical Address.

- Cache Size = 4 KB ($2^{12}$ bytes).

- Block Size = 16 Bytes ($2^4$ bytes).

#### A. Direct Mapping

In Direct Mapping, each block of main memory maps to **exactly one** specific cache line.

- Formula: $Cache\_Line = (Block\_Address) \pmod{Total\_Cache\_Lines}$

**Calculations:**

1. Number of Blocks in RAM = $64KB / 16B = 4096$.

2. Number of Lines in Cache = $4KB / 16B = 256$ lines ($2^8$).

3. **Address Splitting (16 bits):**
   
   - The address is split into Tag, Line Index, and Offset.
   
   - **Word/Byte Offset:** Identifies the specific byte within the 16-byte block. $\log_2(16) = 4$ bits.
   
   - **Line Index:** Identifies which of the 256 lines to check. $\log_2(256) = 8$ bits.
   
   - **Tag:** Used to verify if the data in the line belongs to the requested address. Remaining bits: $16 - 8 - 4 = 4$ bits.

| Tag (4 bits) | Line Index (8 bits) | Word Offset (4 bits) |
| ------------ | ------------------- | -------------------- |

- *Pros:* Simplest hardware; only one comparator needed.

- *Cons:* **Conflict Misses**. If address 0 and address 256 both map to Line 0, they cannot coexist. Alternating access between them causes "thrashing," where they constantly evict each other.

#### B. Associative Mapping (Fully Associative)

A memory block can be placed in **any** free cache line. There is no restriction.

**Address Splitting (16 bits):**

- **Word Offset:** Same as above ($4$ bits).

- **Tag:** Since there is no specific "Line Index" (data can be anywhere), the entire remaining address serves as the tag. $16 - 4 = 12$ bits.

| Tag (12 bits) | Word Offset (4 bits) |
| ------------- | -------------------- |

- *Pros:* Lowest miss rate; no conflict misses because any block can go anywhere.

- *Cons:* **Expensive Hardware**. To find data, the hardware must compare the request tag against *every single line's tag* simultaneously. This requires a massive parallel comparator circuit.

#### C. Set-Associative Mapping (k-way)

This is the industry standard compromise. The cache is divided into Sets. A block maps to a unique Set (like Direct Mapping), but within that Set, it can occupy any of the $k$ lines (like Associative).

- Example: **2-way Set Associative** (Each set contains 2 lines).

**Calculations:**

1. Total Lines = 256.

2. Size of Set = 2 lines ($k=2$).

3. Number of Sets = $256 / 2 = 128$ sets ($2^7$).

4. **Address Splitting (16 bits):**
   
   - **Word Offset:** $4$ bits.
   
   - **Set Index:** Identifies the Set. $\log_2(128) = 7$ bits.
   
   - **Tag:** Remaining bits. $16 - 7 - 4 = 5$ bits.

| Tag (5 bits) | Set Index (7 bits) | Word Offset (4 bits) |
| ------------ | ------------------ | -------------------- |

### 5. Replacement Algorithms

When the Cache (or a specific Set) is full and a miss occurs, the controller must decide which existing block to evict to make room for the new one.

**Example Trace:** Consider a tiny cache with **3 lines** (fully associative) to visualize the behavior.
Reference String (Order of requests): **1, 3, 0, 3, 5, 6**

#### A. FIFO (First In First Out)

Replace the block that entered the cache earliest, regardless of how recently it was used.

| Request | Cache State [Line 0, Line 1, Line 2] | Hit/Miss | Action                                     |
| ------- | ------------------------------------ | -------- | ------------------------------------------ |
| **1**   | [1, -, -]                            | Miss     | Insert 1.                                  |
| **3**   | [1, 3, -]                            | Miss     | Insert 3.                                  |
| **0**   | [1, 3, 0]                            | Miss     | Insert 0. Cache is now **Full**.           |
| **3**   | [1, 3, 0]                            | Hit      | Hit on 3. FIFO order does not change.      |
| **5**   | [5, 3, 0]                            | Miss     | **Evict 1** (it arrived first). Insert 5.  |
| **6**   | [5, 6, 0]                            | Miss     | **Evict 3** (it arrived second). Insert 6. |

*Analysis:* FIFO is easy to implement (circular buffer) but performs poorly because it might throw out a heavily used variable (like a loop counter) just because it was loaded early.

#### B. LRU (Least Recently Used)

Replace the block that has not been accessed for the longest time. This algorithm assumes that if data was used recently, it will be used again (Temporal Locality).

| Request | Cache State | Hit/Miss | Action                                                                     |
| ------- | ----------- | -------- | -------------------------------------------------------------------------- |
| **1**   | [1, -, -]   | Miss     | Insert 1.                                                                  |
| **3**   | [1, 3, -]   | Miss     | Insert 3.                                                                  |
| **0**   | [1, 3, 0]   | Miss     | Insert 0.                                                                  |
| **3**   | [1, 3, 0]   | Hit      | **Update Usage:** 3 is now the Most Recently Used (MRU). Order: 3 > 0 > 1. |
| **5**   | [5, 3, 0]   | Miss     | Who is Least Recently Used? 1. **Evict 1**. Insert 5.                      |
| **6**   | [5, 3, 6]   | Miss     | Current usage age: 5 (new), 3 (recent), 0 (oldest). **Evict 0**. Insert 6. |

*Analysis:* LRU generally performs better but is harder to implement in hardware because the system must constantly track the "age" or usage bit of every cache line.

### 6. Write Policy

Handling memory writes is complex. When the CPU writes data, it writes to the Cache. We must decide when to update the slower Main Memory to ensure data consistency.

#### A. Write-Through

- **Operation:** Every time the CPU writes to the cache, the data is **simultaneously** written to main memory.

- **Pros:** Main memory always holds the most current data. This simplifies data recovery and multi-processor consistency.

- **Cons:** Very slow because every write operation is limited by the speed of the main RAM. High bus traffic.

#### B. Write-Back (Copy-Back)

- **Operation:** The CPU writes **only** to the cache line. The write is not sent to main memory immediately.

- **Dirty Bit:** To track this, every cache line has a "Dirty Bit". When the CPU writes to a line, the Dirty Bit is set to 1.

- **Update:** Main memory is updated ONLY when that specific dirty line is about to be evicted (replaced) to make room for new data.

- **Pros:** Extremely fast; writes occur at cache speed. Multiple writes to the same variable (e.g., `i++` in a loop) are merged into a single memory write later.

- **Cons:** Complex. If power fails before eviction, data is lost. Direct Memory Access (DMA) units reading RAM might get stale data.

#### Allocation Policies on Write Miss

When the CPU tries to write to an address that is NOT in the cache (Write Miss):

1. **Write Allocate:** The block is loaded from RAM into the cache, and then the write is performed. (Typically paired with **Write-Back**).

2. **No-Write Allocate:** The block is NOT loaded into the cache. The write is sent directly to main memory. (Typically paired with **Write-Through**).
