Of course. Here are the detailed, exam-oriented notes for Unit 2, structured to cover the types of questions you've shared.

---

## UNIT-2: Process and Concurrency Management

### 1. Process Management Fundamentals

A **process** is a program in execution. The OS manages the entire lifecycle of a process, from creation to termination, using several key structures and schedulers.

#### Process States

As a process runs, it moves between different states. The five-state model is the most common:

1. **New:** The process is being created.

2. **Ready:** The process is in main memory, waiting for the CPU to be assigned.

3. **Running:** The process's instructions are being executed by the CPU.

4. **Waiting (Blocked):** The process is waiting for an event to happen (e.g., an I/O operation to complete).

5. **Terminated:** The process has finished execution.

---

#### Process Control Block (PCB)

The **Process Control Block (PCB)** is a data structure in the kernel that stores all the information about a specific process. It is the "brain" of the process from the OS's perspective.

**How the PCB helps in Process Management:** The PCB is essential for **context switching**. When the OS needs to switch the CPU from one process (P1) to another (P2), it saves the current state of P1 (all its registers, program counter, etc.) into its PCB. Then, it loads the state of P2 from its PCB into the CPU's registers, allowing P2 to resume execution. Without the PCB, there would be no way to stop and restart processes.

**Key information in a PCB includes:**

- **Process State:** The current state (Ready, Running, etc.).

- **Process ID (PID):** A unique identifier.

- **Program Counter:** The address of the next instruction to execute.

- **CPU Registers:** A snapshot of the process's register values.

- **Memory Management Info:** Page tables or other memory pointers.

- **CPU Scheduling Info:** The process's priority, pointers to scheduling queues.

---

#### Schedulers

- **Long-Term Scheduler (Job Scheduler):** Selects processes from the job pool on the disk and loads them into the ready queue in memory. It controls the **degree of multiprogramming**.

- **Short-Term Scheduler (CPU Scheduler):** Selects a process from the ready queue and allocates the CPU to it. It runs very frequently.

- **Dispatcher:** The dispatcher is the module that gives control of the CPU to the process selected by the short-term scheduler. This involves switching context, switching to user mode, and jumping to the proper location in the user program to restart that program.

---

### 2. CPU Scheduling

CPU scheduling determines which process in the ready queue gets the CPU.

#### Preemptive vs. Non-Preemptive Scheduling

- **Non-Preemptive Scheduling:** Once the CPU has been allocated to a process, it keeps the CPU until it either releases it by terminating or by switching to the waiting state. The OS cannot force the process to give up the CPU.

- **Preemptive Scheduling:** The OS can forcibly take the CPU away from a process. This can happen when its time slice expires (in Round Robin) or when a higher-priority process enters the ready queue.

---

#### CPU Scheduling Algorithms

Here we will use the following example set of processes to illustrate the algorithms:

| **Process** | **Arrival Time** | **Burst Time (ms)** |
| ----------- | ---------------- | ------------------- |
| P1          | 0                | 21                  |
| P2          | 0                | 3                   |
| P3          | 0                | 6                   |
| P4          | 0                | 2                   |

**Key Terms:**

- **Turnaround Time (TAT)** = Completion Time - Arrival Time

- **Waiting Time (WT)** = Turnaround Time - Burst Time

#### First-Come, First-Served (FCFS)

- **Description:** A non-preemptive algorithm where processes are executed in the order they arrive.

- **Gantt Chart:**

- Calculation:
  
  | Process | WT | TAT |
  
  | :--- | :- | :-- |
  
  | P1 | 0 | 21 |
  
  | P2 | 21 | 24 |
  
  | P3 | 24 | 30 |
  
  | P4 | 30 | 32 |
  
  | Total| 75 | 107|
  
  - **Average Waiting Time** = $75 / 4 = 18.75$ ms
  
  - **Average Turnaround Time** = $107 / 4 = 26.75$ ms

---

#### Shortest-Remaining-Time-First (SRTF)

- **Description:** The preemptive version of Shortest-Job-First (SJF). The CPU is allocated to the process with the smallest remaining burst time. If a new process arrives with a shorter burst time than what is left for the currently running process, the current process is preempted.

---

#### Round Robin (RR)

- **Description:** A preemptive algorithm designed for time-sharing systems. Each process gets a small unit of CPU time (a **time quantum**). When the time is up, the process is preempted and added to the end of the ready queue.

---

### 3. Concurrency and Synchronization

#### Inter-Process Communication (IPC)

IPC provides mechanisms for cooperating processes to communicate and synchronize their actions, typically for sharing data, speeding up computation, or modularity.

#### The Critical Section Problem

A **critical section** is a part of a program that accesses a shared resource. The problem is to design a protocol that processes can use to cooperate. A solution must provide:

1. **Mutual Exclusion:** Only one process can be in its critical section at a time.

2. **Progress:** If no process is in its critical section, a process that wants to enter should not be blocked indefinitely.

3. **Bounded Waiting:** There's a limit to how many times other processes can enter their critical sections after a process has requested entry.

---

#### Semaphore

A **semaphore** is an integer variable used for controlling access to a common resource by multiple processes. It can only be accessed via two atomic operations:

- `wait(S)` or `P(S)`: Decrements the semaphore value. If the value becomes negative, the process is blocked.

- `signal(S)` or `V(S)`: Increments the semaphore value. If there are processes blocked on this semaphore, one is unblocked.

---

### 4. Deadlock

A **deadlock** is a situation where a set of processes are blocked because each process is holding a resource and waiting for another resource acquired by some other process.

#### Necessary Conditions for Deadlock

All four of these conditions must hold for a deadlock to occur:

1. **Mutual Exclusion:** At least one resource is held in a non-sharable mode.

2. **Hold and Wait:** A process holds at least one resource and is waiting for another.

3. **No Preemption:** A resource can only be released voluntarily by the process holding it.

4. **Circular Wait:** A chain of processes exists where each process is waiting for a resource held by the next process in the chain.

---

#### Methods of Deadlock Handling

##### (a) Deadlock Prevention

Prevent deadlocks by ensuring that at least one of the four necessary conditions can never hold.

- **Break Mutual Exclusion:** Make resources sharable (not always possible).

- **Break Hold and Wait:** Require a process to request all resources at once.

- **Break No Preemption:** Allow the OS to preempt resources from a process.

- **Break Circular Wait:** Impose a total ordering on all resource types and require processes to request them in that order.

##### (b) Deadlock Avoidance

The OS uses prior information about the maximum resources a process may need to ensure the system never enters an **unsafe state** (a state that could lead to a deadlock).

- **Banker's Algorithm:** This is the most famous deadlock avoidance algorithm. When a process requests resources, the algorithm checks if granting the request would keep the system in a safe state. If it would, the resources are allocated; otherwise, the process must wait.

##### (c) Deadlock Detection and Recovery

This approach lets deadlocks occur, then runs an algorithm to detect them and a recovery scheme to fix them.

- **Detection:** Use a **Wait-For Graph** to detect cycles, which indicate a deadlock.

- **Recovery:**
  
  - **Process Termination:** Abort one or more of the deadlocked processes.
  
  - **Resource Preemption:** Forcibly take a resource from one process and give it to another. This often requires rolling the preempted process back to a safe state.

---

### 5. Multithreading

A **thread** is a lightweight unit of execution within a process. A single process can have multiple threads, which share the process's code, data, and files but have their own registers, stack, and program counter.

- **Benefits:** Increased responsiveness, efficient resource sharing, and better performance on multi-core systems.

- **Models:**
  
  - **User-Level Threads:** Managed by a user-level library without kernel support. Fast but a blocking call blocks the whole process.
  
  - **Kernel-Level Threads:** Managed directly by the OS. Slower to create but a blocking call won't block the entire process.
