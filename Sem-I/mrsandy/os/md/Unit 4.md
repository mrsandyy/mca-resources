# Unit 4: File and Disk Management

## 1. File Concepts

### Definition of a File

A **file** is a **named collection of related information** that is recorded on secondary storage (like a hard drive or SSD).

From the user's perspective, it's the smallest logical unit for storing data (e.g., a document, a photo, a program). The operating system (OS) abstracts away the physical properties of the storage device to present a uniform, **logical view of data**, meaning you don't have to worry about the physical details of how or where it's stored.

### File Attributes

These are metadata (information *about* the file) stored by the file system, separate from the data itself.

- **Name:** The human-readable identifier (e.g., `report.pdf`).

- **Identifier:** A unique system-generated tag (often a number) that identifies the file *within* the file system.

- **Type:** The file's category, often used by the OS to know which program to open it with (e.g., `.exe`, `.txt`).

- **Location:** A pointer to the device and the file's exact location on that device.

- **Size:** The current size of the file (in bytes, KB, MB, etc.).

- **Protection:** Access control information—who can **read**, **write**, or **execute** the file (e.g., user, group, public permissions).

- **Timestamps:**
  
  - **Creation time:** When the file was created.
  
  - **Last modification time:** When the file's *content* was last changed.
  
  - **Last access time:** When the file was last read.

### File Types

File types, often identified by an **extension** (like `.exe` or `.pdf`), help the operating system and users understand how to use the file. This allows the OS to perform appropriate actions, like running an executable or opening a document in a word processor.

| **File Type**      | **Extensions**       | **Description / Purpose**                   |
| ------------------ | -------------------- | ------------------------------------------- |
| **Executable**     | .exe, .com, .bin     | Ready-to-run machine language programs.     |
| **Object**         | .obj, .o             | Compiled machine code, not yet linked.      |
| **Source Code**    | .c, .java, .asm, .py | Source programs in various languages.       |
| **Batch**          | .bat, .sh            | Command scripts for command interpreters.   |
| **Text**           | .txt, .doc, .md      | Plain text or document files.               |
| **Word Processor** | .wp, .rtf, .doc      | Word processing document formats.           |
| **Library**        | .lib, .so, .dll      | Libraries of routines for programmers.      |
| **Print/View**     | .ps, .pdf, .jpg      | Files formatted for printing or viewing.    |
| **Archive**        | .zip, .tar, .rar     | Related files grouped and often compressed. |
| **Multimedia**     | .mpeg, .mp3, .avi    | Audio or audio/video (A/V) files.           |

## 2. File Access Methods

This defines *how* the data within a file is read or written.

### 1. Sequential Access

- **Concept:** This is the simplest access method. Information in the file is processed **in order, one record after another**, from beginning to end.

- **Operations:** A file pointer tracks the current read/write position. Read and write operations automatically move this pointer to the next position. You can also `reset_to_beginning()`.

- **Example:** Reading a `.txt` file or streaming a video. This models old tape-based systems but is still very common.

### 2. Direct Access (or Random/Relative Access)

- **Concept:** The file is viewed as a numbered sequence of logical blocks or records. You can **read or write any block directly in any order** by using its block number, without having to read the ones before it.

- **Operations:** `read_block(n)`, `write_block(n)`, `jump_to_block(n)`.

- **Example:** This is essential for **databases**. If you need to retrieve customer record #1050, you can jump directly to that record without reading records 1-1049 first.

### 3. Indexed Access

- **Concept:** This method enhances direct access by using an **index** (like the index at the back of a book) that stores pointers to the actual data blocks.

- **How it works:** To find a specific piece of data, you first look it up in the (smaller) index file, which gives you a pointer to the *actual* data block in the (larger) main file. This allows for fast searching without scanning the entire file.

- **Example:** A large database file might have an index based on a key (like a User ID).

- **ISAM (Indexed Sequential Access Method)** is an example that uses a small master index pointing to a secondary index, which then points to the data blocks. This allows a record to be found with very few reads. For large files, multi-level indexes can be used.

## 3. Directory and File System Structure

### Directory Structure

Storage devices (like disks) are often divided into **partitions** or **volumes**. Each volume can contain a separate file system. A **directory** (or "folder") is a special file present on each volume that stores information about the files and other directories it contains, such as their name, type, size, and location.

- **Single-Level Directory:**
  
  - **Concept:** One single directory for *all* files from *all* users.
  
  - **Problem:** Naming conflicts (two users can't both have a `test.c` file) and poor organization. Only used in very simple, old systems.

- **Two-Level Directory:**
  
  - **Concept:** A **Master File Directory (MFD)** contains a list of **User File Directories (UFDs)**. Each user gets their own UFD.
  
  - **Solves:** The naming conflict problem. User A and User B can both have a `test.c`.
  
  - **Problem:** Users can't easily share files, and it's still hard to organize (a user can't create sub-folders).

- **Tree-Structured Directory (Most Common):**
  
  - **Concept:** A true hierarchy. Each directory can contain both files and other *sub-directories*.
  
  - There is one **root** directory. All files and directories are descendants of the root.
  
  - **Pathnames:** A file is uniquely identified by its **path** from the root.
    
    - **Absolute Path:** The path from the root (e.g., `/home/user/docs/report.pdf`).
    
    - **Relative Path:** The path from the current working directory (e.g., if you are in `/home/user`, the relative path is `docs/report.pdf`).

- **Acyclic-Graph Directory:**
  
  - **Concept:** Like a tree, but directories can **share** files or sub-directories. This is done using **links** or **shortcuts**.
  
  - **Acyclic** means there are no cycles (a directory cannot be its own ancestor). This prevents infinite loops in path lookups.
  
  - **Example:** A file `report.pdf` might exist in `/users/alice/projects` but also be *linked* into `/users/bob/shared`. It's the *same* file in both places.

- **General Graph Directory:**
  
  - **Concept:** Allows cycles.
  
  - **Problem:** Very complex. Searching for a file can result in an infinite loop. Requires "garbage collection" to find and delete files when all links to them are gone. rarely used.

### File System Organization (Layered Structure)

File systems are often implemented in layers to separate concerns.

1. **Application Program:** (e.g., your text editor). It just wants to "open," "read," or "write" a file by its name.

2. **Logical File System:** Manages the high-level file structure, metadata, directories, and file protection. It handles directory lookups and translates file names into internal identifiers.

3. **File Organization Module:** Knows *how* files are stored. It translates logical block numbers (e.g., "block 5 of this file") into physical block numbers (e.g., "block 782 on the disk"). This layer manages file allocation (contiguous, linked, indexed) and free space.

4. **Basic File System:** The "driver." It takes commands from the layer above (like "read physical block 782") and issues generic commands to the device driver.

5. **I/O Control (Device Drivers):** This is the software that speaks directly to the hardware controller. It translates generic commands into specific hardware instructions (like "move head to track 50, sector 10").

6. **Devices:** The physical hardware (HDD, SSD).

### Virtual File System (VFS)

VFS is an abstraction layer in the OS that provides a **common interface** for all file systems. This separates generic file operations (like `open()`, `read()`) from their specific implementations.

- **Advantage:** It allows the user to transparently access different types of file systems (e.g., a local disk, a remote network drive) without needing to know the underlying details.

- It uses a `vnode` structure to represent any active file or directory consistently.

## 4. Allocation Methods

Allocation methods determine how disk blocks are allocated to files.

### 1. Contiguous Allocation

- **Concept:** The file is stored in a single, **contiguous** (unbroken) set of blocks on the disk.

- **Directory Entry:** Stores the **start block** and the **length** (number of blocks).

- **Example:** A file `mail` starting at block 19 with length 6 would occupy blocks 19, 20, 21, 22, 23, and 24.

- **Pros:**
  
  - **Excellent performance:** Great for sequential access (just one seek to the start block) and fast for direct access (calculating `start_block + n` is trivial).

- **Cons:**
  
  - **External Fragmentation:** Over time, the disk becomes full of small, unusable holes, just like with memory.
  
  - **Difficult to grow files:** If a file needs to get bigger, there might be no free space immediately after it. You'd have to move the entire file.

### 2. Linked Allocation

- **Concept:** The blocks of a file are **scattered** anywhere on the disk. Each block contains a **pointer** (link) to the *next* block in the file.

- **Directory Entry:** Stores only the **start block**. The last block has a null pointer.

- **Example:** A file's blocks might be `10 -> 25 -> 4 -> 17 -> NULL`.

- **Pros:**
  
  - **No external fragmentation:** Any free block can be used.
  
  - **Files can grow easily:** Just grab any free block and link it to the end.

- **Cons:**
  
  - **Bad for sequential access:** You have to read *each block* to find the *next* block, which can mean many seeks if the blocks are scattered.
  
  - **No direct access:** To get to block 5, you *must* read blocks 1, 2, 3, and 4 first to follow the chain.
  
  - **Poor reliability:** If one pointer in the chain is corrupted, you lose the rest of the file.

- **Variation (FAT):** A **File Allocation Table (FAT)** is used. This is a special table at the beginning of the disk. The pointers are *moved* from the blocks *into* this table. The directory entry points to the first block's entry in the FAT, which then points to the next, and so on. This is much better, as all pointers are in one place (good for caching) and it allows for direct access (by walking the FAT table, which is faster than walking the disk).

### 3. Indexed Allocation

- **Concept:** A compromise that provides direct access without external fragmentation. All the pointers to a file's data blocks are gathered into one special block called an **index block** (or "inode").

- **Directory Entry:** Stores the address of the **index block**.

- **Example:** To create a file, the OS finds a free index block (e.g., block 100). If the file needs 3 data blocks, the OS finds 3 free blocks (e.g., 20, 5, 50) and puts their addresses into the index block: `Index Block (Block 100): [20, 5, 50, ...]`.

- **Pros:**
  
  - **No external fragmentation.**
  
  - **Supports fast direct access.**

- **Cons:**
  
  - **Wasted space:** A very small file (e.g., 1 block) still needs an entire index block, which is wasteful.
  
  - **File size limit:** The number of pointers in one index block limits the file size.

- **Solution to Size Limit:** **Multi-Level Indexing** (used by Linux `ext` file systems). The main index block can contain pointers. Some point to data, but others might point to a block of *nothing but more pointers* (indirect block), which can point to *another* block of pointers (double-indirect), etc.

## 5. Free Space Management

The file system must keep track of all free (unallocated) disk blocks.

- **Bit Vector (or Bitmap):**
  
  - **Concept:** A list of bits, one for each block on the disk. For example: `0` = free block, `1` = allocated block (or the reverse).
  
  - **Example:** `0011110011...` means blocks 2, 3, 4, 5, 8, 9, etc., are free.
  
  - **Pro:** Simple and efficient to find a contiguous block of `n` free blocks.
  
  - **Con:** Can be large. A 1TB disk with 4KB blocks needs 32MB for the bitmap, which must be kept in memory.

- **Linked List:**
  
  - **Concept:** All *free* blocks are linked together. A single pointer in the OS points to the `head` of the free list. Each free block points to the next free block.
  
  - **Pro:** Simple, no extra disk space needed (pointers are in the free blocks).
  
  - **Con:** Inefficient. To find `n` free blocks, you have to read `n` blocks, which is slow. Not efficient for finding *contiguous* blocks.

- **Grouping:**
  
  - **Concept:** A variation of the linked list. The first free block stores the addresses of `n-1` *other* free blocks. The last of these `n-1` blocks points to the *next* group of free blocks.
  
  - **Pro:** You can find a large number of free blocks with one I/O.

- **Counting:**
  
  - **Concept:** Because free blocks are often contiguous, the system stores the address of the *first* free block and the *count* of contiguous free blocks that follow.
  
  - **Example:** The list might look like `(Block: 10, Count: 4)`, `(Block: 50, Count: 20)`.

## 6. Disk Structure

### Physical View (Hardware)

- **Platters:** The circular, magnetic-coated disks that store data.

- **Spindle:** The central axle that the platters rotate around (e.g., at 7200 RPM).

- **Read/Write Heads:** One per platter *surface*. They fly just above the surface, reading or writing data. They move together on an **actuator arm**.

- **Tracks:** Concentric circles on a platter surface.

- **Cylinder:** A set of all tracks that are at the same distance from the center (i.e., all tracks under all the heads at a given position).

- **Sector:** A subdivision of a track. The smallest *addressable* unit of a disk, typically 512 bytes or 4KB.

### Logical View (OS)

- The OS doesn't want to know about `(cylinder, head, sector)`. This is too complex.

- The OS views the *entire* disk as one long, 1-dimensional array of **Logical Blocks** (e.g., Block 0 to Block 1,000,000).

- This is called **Logical Block Addressing (LBA)**.

- The disk's hardware controller is responsible for translating a request for "LBA 400" into the correct `(cylinder, head, sector)` address.

### Formatting (Disk Initialization)

- **Low-Level Formatting (Physical Formatting):** Done at the factory. This creates the physical sectors on the disk and fills them with a default data structure (header, data area, error-correction codes).

- **Partitioning:** Dividing the single logical disk (e.g., LBA 0 to 1,000,000) into one or more virtual "partitions" (e.g., C: drive and D: drive).

- **High-Level Formatting (Logical Formatting):** This is what you do when you "format" a drive. The OS **writes an empty file system structure** onto the partition. This includes creating the free-space management structures (like the bitmap) and the root directory.

## 7. Disk Head Scheduling

Because moving the physical read/write head (a **seek**) is the *slowest* part of disk I/O, the OS tries to optimize this. It keeps a **queue** of pending I/O requests for a disk to decide which request to service next.

**Example Scenario:**

- **Request Queue:** `98, 183, 37, 122, 14, 124, 65, 67`

- **Head Starts At:** `53`

- **Total Tracks:** 0-199

### 1. FCFS (First-Come, First-Served)

- **Algorithm:** Process requests in the order they arrive.

- **Movement:** `53 → 98 → 183 → 37 → 122 → 14 → 124 → 65 → 67`

- **Total Head Movement:** (98-53) + (183-98) + (183-37) + (122-37) + (122-14) + (124-14) + (124-65) + (67-65) = **640 cylinders**

- **Pro:** Fair and simple.

- **Con:** Extremely inefficient.

### 2. SSTF (Shortest Seek Time First)

- **Algorithm:** Service the request that is *closest* to the head's current position.

- **Movement:** `53 → 65 → 67 → 37 → 14 → 98 → 122 → 124 → 183`

- **Total Head Movement:** (65-53) + (67-65) + (67-37) + (37-14) + (98-14) + (122-98) + (124-122) + (183-124) = **236 cylinders**

- **Pro:** Very efficient, much better than FCFS.

- **Con:** **Starvation**. New requests that are close to the head will keep getting serviced, while a request far away might *never* be serviced.

### 3. SCAN (Elevator Algorithm)

- **Algorithm:** The head moves from one end of the disk to the other (e.g., toward 0), servicing requests as it goes. When it hits the end, it reverses direction.

- **Movement (assuming moving towards 0 first):** `53 → 37 → 14 → 0` (hits end) `→ 65 → 67 → 98 → 122 → 124 → 183`

- **Total Head Movement:** (53-0) + (183-0) = **236 cylinders**

- **Pro:** Good performance, avoids starvation.

- **Con:** Unfair. A request that *just* missed the head has to wait for a full sweep.

### 4. C-SCAN (Circular SCAN)

- **Algorithm:** A modification of SCAN. The head *only* services requests when moving in one direction. When it hits the end, it does a **fast seek** all the way back to the other end *without* servicing requests, and then starts its sweep again.

- **Movement (assuming moving towards 199):** `53 → 65 → 67 → 98 → 122 → 124 → 183 → 199` (hits end) `→ 0` (fast seek) `→ 14 → 37`

- **Total Head Movement:** (199-53) + (199-0) + (37-0) = **382 cylinders**

- **Pro:** Provides a more uniform and fair wait time than SCAN.

### 5. LOOK and C-LOOK

- **Algorithm:** Optimized versions of SCAN and C-SCAN. They are identical, except the head **LOOKs** ahead for requests. If there are no more requests in the current direction, it reverses *immediately*—it doesn't go all the way to the end (track 0 or 199) for no reason.

- **LOOK Example (like SCAN):** `53 → 37 → 14` (reverses, doesn't go to 0) `→ 65 → 67 → 98 → 122 → 124 → 183`

- **C-LOOK Example (like C-SCAN):** `53 → 65 → 67 → 98 → 122 → 124 → 183` (reverses) `→ 14 → 37`

- **Pro:** Most practical and efficient. This is what most modern OSes use.

### Selecting an Algorithm

- SSTF or LOOK are often good defaults.

- SCAN and C-SCAN are better for systems with a heavy disk load to prevent starvation.

- Performance also depends on the file allocation method; contiguous files (Contiguous Allocation) will naturally reduce head movement.

## 8. Swap Management

- **What is it?** This is how the OS manages the **swap space** (also called the paging file) on the disk, which is used for **Virtual Memory**.

- **Purpose:** When physical memory (RAM) is low, the OS moves processes or, more commonly, individual **pages** from memory to the swap space on the disk. This is called **swapping**.

- **Where is it?** It can be a regular file *inside* the file system, or (more commonly for performance) a separate, dedicated raw partition.

- **How is it managed?** The OS needs to be able to swap pages in and out *very quickly*.
  
  - If it's a raw partition, the OS can use a much simpler, faster "swap-space manager" that just needs to find blocks of a certain size. It often uses a **bitmap** similar to the free-space manager, as it needs to find *contiguous* blocks to write out pages quickly.
  
  - Swap I/O is typically faster than standard file system I/O because it is allocated in large, contiguous chunks.
