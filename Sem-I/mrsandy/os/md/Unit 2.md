## UNIT-2: Process and Concurrency Management

### Process Management Fundamentals

A **process** is a program in execution. It's more than just the program code; it also includes the current activity, represented by the value of the **program counter**, the contents of the processor's **registers**, the **process stack** (containing temporary data like function parameters and local variables), and a **data section** (containing global variables).

#### Process States

As a process executes, it changes state. The state of a process is defined by its current activity. A process can be in one of the following five states:

1. **New:** The process is being created.

2. **Ready:** The process has been loaded into main memory and is waiting to be assigned to a processor.

3. **Running:** Instructions are being executed by the CPU.

4. **Waiting (or Blocked):** The process is waiting for some event to occur, such as the completion of an I/O operation or receiving a signal.

5. **Terminated:** The process has finished execution.

These states change as follows:

- **New → Ready:** Admitted into the ready queue.

- **Ready → Running:** The scheduler dispatches the process to the CPU.

- **Running → Ready:** An interrupt or time slice expiry occurs.

- **Running → Waiting:** The process requests I/O or waits for an event.

- **Running → Terminated:** The process completes its execution.

- **Waiting → Ready:** The I/O or event the process was waiting for has completed.

#### Process Control Block (PCB)

For each process, the operating system maintains a **Process Control Block (PCB)**, which is a data structure containing all the information needed to manage that specific process. It is the "brain" of the process from the OS's perspective.

**How the PCB helps in Process Management:** The PCB is essential for **context switching**. When the OS needs to switch the CPU from one process (P1) to another (P2), it saves the current state of P1 (all its registers, program counter, etc.) into its PCB. Then, it loads the state of P2 from its PCB into the CPU's registers, allowing P2 to resume execution. Without the PCB, there would be no way to stop and restart processes.

The PCB contains:

- **Process State:** The current state (New, Ready, Running, etc.).

- **Process ID (PID):** A unique identifier for the process.

- **Program Counter (PC):** Indicates the address of the next instruction to be executed for this process.

- **CPU Registers:** A snapshot of all CPU registers (accumulators, index registers, etc.) that the process was using.

- **CPU-Scheduling Information:** Process priority, pointers to scheduling queues.

- **Memory-Management Information:** Pointers to the page tables or segment tables for the process.

- **Accounting Information:** CPU time used, time limits, etc.

- **I/O Status Information:** List of I/O devices allocated to the process, a list of open files.

#### Schedulers

A scheduler is an OS module that selects which process should run next.

1. **Long-Term Scheduler (or Job Scheduler):**
   
   - Selects processes from a pool on the disk (the job pool) and loads them into main memory for execution.
   
   - It runs infrequently (seconds or minutes).
   
   - Its main purpose is to control the **degree of multiprogramming** (the number of processes in memory).

2. **Short-Term Scheduler (or CPU Scheduler):**
   
   - Selects a process from among those that are ready in memory and allocates the CPU to it.
   
   - It must be very fast, as it runs very frequently (milliseconds).

3. **Medium-Term Scheduler:**
   
   - This scheduler is involved in **swapping**. It can remove a process from memory (and from active contention for the CPU) and swap it out to disk. The process can be swapped back in later to continue from where it left off.
   
   - This is used to reduce the degree of multiprogramming temporarily, perhaps to free up memory.

4. **Dispatcher:**
   
   - The dispatcher is the module that gives control of the CPU to the process selected by the short-term scheduler. This involves switching context, switching to user mode, and jumping to the proper location in the user program to restart that program.

### CPU Scheduling

CPU scheduling deals with the problem of deciding which of the processes in the ready queue is to be allocated the CPU.

#### Preemptive vs. Non-Preemptive Scheduling

- **Non-Preemptive Scheduling:** Once the CPU has been allocated to a process, it keeps the CPU until it either releases it by terminating or by switching to the waiting state. The OS cannot force the process to give up the CPU.

- **Preemptive Scheduling:** The OS can forcibly take the CPU away from a process. This can happen when its time slice expires (in Round Robin) or when a higher-priority process enters the ready queue.

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

The simplest scheduling algorithm. The process that requests the CPU first is allocated the CPU first.

- **Nature:** Non-preemptive.

- **Pros:** Very simple to understand and implement.

- **Cons:** Average waiting time can be very long. Suffers from the **convoy effect**, where a long process can make all subsequent short processes wait.

- **Gantt Chart (for example):**
  
  ```
  |    P1    |  P2 |   P3   | P4 |
  0         21   24       30   32
  ```

- **Calculation:** | Process | WT | TAT |
  | :--- | :- | :-- |
  | P1 | 0 | 21 |
  | P2 | 21 | 24 |
  | P3 | 24 | 30 |
  | P4 | 30 | 32 |
  | Total| 75 | 107|
  
  - **Average Waiting Time** = $75 / 4 = 18.75$ ms
  
  - **Average Turnaround Time** = $107 / 4 = 26.75$ ms

#### Shortest-Job-First (SJF)

The CPU is allocated to the process that has the smallest next CPU burst.

- **Two Types:**
  
  1. **Non-preemptive SJF:** Once the CPU is given to a process, it cannot be preempted until it completes its CPU burst.
  
  2. **Preemptive SJF (also called Shortest-Remaining-Time-First, SRTF):** If a new process arrives with a CPU burst length less than the remaining time of the current executing process, it is preempted.

- **Pros:** Provably optimal – gives the minimum average waiting time for a given set of processes.

- **Cons:** The main challenge is knowing the length of the next CPU burst. It's impossible to know in advance and must be predicted.

#### Priority Scheduling

A priority number is associated with each process. The CPU is allocated to the process with the highest priority.

- **Two Types:** Preemptive and Non-preemptive.

- **Problem:** **Starvation** or indefinite blocking. A low-priority process might never get to run.

- **Solution:** **Aging**, a technique of gradually increasing the priority of processes that wait in the system for a long time.

#### Round Robin (RR)

Designed especially for timesharing systems. It's similar to FCFS but with preemption.

- **How it works:** A small unit of time, called a **time quantum** or **time slice** (typically 10-100 milliseconds), is defined. The ready queue is treated as a circular queue. The CPU scheduler goes around the ready queue, allocating the CPU to each process for a time interval of up to 1-time quantum.

- **Nature:** Preemptive.

- **Performance:** Heavily depends on the size of the time quantum.
  
  - If the quantum is too large, RR becomes FCFS.
  
  - If the quantum is too small, the overhead of context switching becomes too high.

### Inter-Process Communication (IPC)

**IPC** refers to the mechanisms an OS provides to allow processes to communicate with each other and to synchronize their actions.

#### Need for IPC

- **Information Sharing:** Multiple processes may need to access the same piece of information (e.g., a shared file).

- **Computation Speedup:** A task can be broken down into sub-tasks that run in parallel on different processes, speeding up the total execution time.

- **Modularity:** A system can be built as a set of cooperating processes, making it easier to design and manage.

- **Convenience:** A user may want to work on multiple tasks at the same time (e.g., editing, printing, and compiling).

### Concurrency and Synchronization

#### The Critical Section Problem

When multiple processes cooperate, they may share common data. A **critical section** is a segment of code in a process where shared resources are accessed. The key challenge is to ensure that when one process is executing in its critical section, no other process is allowed to execute in its critical section for the same shared resource.

A solution to the critical section problem must satisfy three requirements:

1. **Mutual Exclusion:** If a process is executing in its critical section, then no other processes can be executing in their critical sections.

2. **Progress:** If no process is executing in its critical section and some processes wish to enter, then only those processes that are not executing in their remainder sections can participate in the decision on which will enter its critical section next, and this selection cannot be postponed indefinitely.

3. **Bounded Waiting:** There must be a bound on the number of times that other processes are allowed to enter their critical sections after a process has made a request to enter its critical section and before that request is granted.

#### Semaphore

A **semaphore** is an integer variable used for controlling access to a common resource by multiple processes. It can only be accessed via two atomic operations:

- `wait(S)` or `P(S)`: Decrements the semaphore value. If the value becomes negative, the process is blocked.

- `signal(S)` or `V(S)`: Increments the semaphore value. If there are processes blocked on this semaphore, one is unblocked.

**Types of Semaphores:**

1. **Counting Semaphore:** The value can range over an unrestricted domain. Used to control access to a resource with multiple instances. The semaphore is initialized to the number of available resources.

2. **Binary Semaphore (or Mutex):** The value can only be 0 or 1. It behaves like a lock, providing mutual exclusion.

#### Classical Problems in Concurrent Programming

- **The Producer-Consumer Problem (or Bounded-Buffer Problem):** There is a buffer of fixed size. A producer process produces items and places them in the buffer. A consumer process consumes items from the buffer. The problem is to ensure that the producer doesn't try to add data into the buffer if it's full and the consumer doesn't try to remove data from an empty buffer.

- **The Readers-Writers Problem:** A database is shared among several concurrent processes. Some of these processes may only want to read the database (readers), and some may want to update it (writers). The problem is to allow multiple readers to read at the same time, but only one writer at any time can have access.

### Deadlock

A **deadlock** is a situation where two or more processes are blocked forever, each waiting for a resource that is held by another process in the set.

#### Deadlock Characteristics (Necessary Conditions)

A deadlock can arise if and only if the following four conditions hold simultaneously in a system:

1. **Mutual Exclusion:** At least one resource must be held in a non-sharable mode; that is, only one process at a time can use the resource.

2. **Hold and Wait:** A process must be holding at least one resource and waiting to acquire additional resources that are currently being held by other processes.

3. **No Preemption:** Resources cannot be preempted; that is, a resource can be released only voluntarily by the process holding it.

4. **Circular Wait:** A set of waiting processes {P₀, P₁, ..., Pₙ} must exist such that P₀ is waiting for a resource held by P₁, P₁ is waiting for a resource held by P₂, ..., Pₙ is waiting for a resource held by P₀.

#### Deadlock Handling Strategies

##### (a) Deadlock Prevention

Prevent deadlocks by ensuring that at least one of the four necessary conditions can never hold.

- **Break Mutual Exclusion:** Make resources sharable (not always possible).

- **Break Hold and Wait:** Require a process to request all resources at once, or release all resources before requesting new ones. This can lead to low resource utilization.

- **Break No Preemption:** If a process holding resources requests another resource that cannot be immediately allocated, it must release all resources it is currently holding.

- **Break Circular Wait:** Impose a total ordering of all resource types and require that each process requests resources in an increasing order of enumeration.

##### (b) Deadlock Avoidance

Requires that the operating system be given additional information in advance concerning which resources a process will request. The OS uses this information to decide whether a request can be satisfied or must be delayed to ensure the system never enters an **unsafe state**. An unsafe state may lead to a deadlock.

- **Banker's Algorithm** is a classic deadlock avoidance algorithm. It works by simulating the allocation of resources to determine if it would leave the system in a safe state.

##### (c) Deadlock Detection and Recovery

If a system does not employ either a deadlock prevention or avoidance algorithm, then a deadlock situation may occur. This approach involves:

1. **Detection:** The system runs an algorithm to determine if a deadlock has occurred. This can be done using a **Wait-For Graph**.

2. **Recovery:** Once a deadlock is detected, the system must recover.
   
   - **Process Termination:** Abort one or more processes to break the circular wait.
   
   - **Resource Preemption:** Take a resource from one process and give it to another. This may involve **rollback**, where the preempted process is returned to a previous safe state.

### Multithreading

A **thread** is a basic unit of CPU utilization. It is a "lightweight process" that comprises a thread ID, a program counter, a register set, and a stack. A thread shares with other threads belonging to the same process its code section, data section, and other operating-system resources, such as open files. A traditional process has a single thread of control.

#### Benefits of Multithreading

- **Responsiveness:** An interactive application can remain responsive to the user even if a part of it is blocked or performing a lengthy operation.

- **Resource Sharing:** Threads share the memory and resources of the process they belong to by default, which is more efficient than sharing memory between processes.

- **Economy:** It is much cheaper (less time and memory) to create and switch between threads than processes.

- **Scalability:** A multi-threaded application can take advantage of multiprocessor architectures by running threads in parallel on different processors.

#### User-Level Threads vs. Kernel-Level Threads

- **User-Level Threads:** Managed by a thread library in user space (e.g., POSIX Pthreads, Java threads). The kernel is unaware of them.
  
  - **Pros:** Fast to create and manage because no system calls are needed.
  
  - **Cons:** If one thread makes a blocking system call, the entire process will block, even if other threads are ready to run. Cannot take advantage of multiprocessing.

- **Kernel-Level Threads:** Supported and managed directly by the operating system.
  
  - **Pros:** The kernel can schedule threads on different processors. If one thread is blocked, the kernel can schedule another thread from the same process to run.
  
  - **Cons:** Slower to create and manage than user threads because they require kernel intervention.
