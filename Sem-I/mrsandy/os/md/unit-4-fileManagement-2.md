# 📂 File Concepts

### Definition of a File

A file is a **named collection of related information** that is stored on secondary storage (like a hard drive)1. It provides a **logical view of data**, meaning you don't have to worry about the physical details of how or where it's stored2. Files can store programs or data in various formats, such as text, binary, or images3. It is the smallest logical unit for data storage and management4.

### File Attributes

Every file has attributes that define its properties5. These include:

- **Identifier:** A unique system-generated tag to distinguish the file6.

- **Type:** Specifies the file's format (e.g., text, executable)7.

- **Location:** The exact position of the file on the storage device8.

- **Size:** The current size of the file9.

- **Protection:** Permissions controlling who can read, write, or execute the file10.

- **Time, Date, and User ID:** Records of creation, modification, and last access for security and tracking11.

### File Types

File types, often identified by an extension (like `.exe` or `.pdf`), help the operating system and users understand how to use the file12. This allows the OS to perform appropriate actions, like running an executable or opening a document in a word processor13.

| **File Type**      | **Extensions**    | **Description / Purpose**                     |
| ------------------ | ----------------- | --------------------------------------------- |
| **Executable**     | .exe, .com, .bin  | Ready-to-run machine language programs14.     |
| **Object**         | .obj, .o          | Compiled machine code, not yet linked15.      |
| **Source Code**    | .c, .java, .asm   | Source programs in various languages16.       |
| **Batch**          | .bat, .sh         | Command scripts for command interpreters17.   |
| **Text**           | .txt, .doc        | Plain text or document files18.               |
| **Word Processor** | .wp, .rtf, .doc   | Word processing document formats19.           |
| **Library**        | .lib, .so, .dll   | Libraries of routines for programmers20.      |
| **Print/View**     | .ps, .pdf, .jpg   | Files for printing or viewing21.              |
| **Archive**        | .zip, .tar        | Related files grouped and often compressed22. |
| **Multimedia**     | .mpeg, .mp3, .avi | Audio or audio/video files23.                 |

---

## ➡️ File Access Methods

File access methods define how information stored in a file can be read into memory24. The main types are Sequential, Direct, and Index-based.

### 1. Sequential Access

This is the simplest access method, where data is processed in order, one record after another25. It follows a tape-like model26. Read and write operations automatically move a file pointer to the next position27.

28

### 2. Direct Access (Random Access)

In direct access, blocks or records can be accessed in **any order** by using their block number29. The file is viewed as a numbered sequence of blocks30. This method is essential for applications like databases, where quick access to a specific piece of data (e.g., a specific flight record) is required without reading the whole file31.

- Operations are specified as `Read(n)` or `Write(n)`, where `n` is the block number32.

- Sequential access can be simulated on a direct-access file by maintaining a "current position" variable, but the reverse is very inefficient33333333.

### 3. Other Methods (Index-Based Access)

This method enhances direct access by using an **index** that stores pointers to the actual data blocks34. To find a record, the system first searches the smaller index file and then uses the pointer to directly access the correct data block35.

- This allows for fast searching without scanning the entire file36.

- For large files, multi-level indexes can be used37.

- **ISAM (Indexed Sequential Access Method)** is an example that uses a small master index pointing to a secondary index, which then points to the data blocks38. This allows a record to be found with very few reads39.

40

---

## 🗂️ Directory and File System Structure

### Directory Structure

Storage devices (like disks) are often divided into **partitions** or **volumes**41. Each volume can contain a separate file system42. A **directory** is present on each volume to store information about the files it contains, such as their name, type, size, and location43434343.

44

### File System Organization

The file system is organized in layers, from the low-level device drivers to the high-level logical file management45.

- **Data Transfer:** Data is transferred in fixed-size **blocks** (e.g., 512 bytes)46.

- **Access:** Disks allow **direct access**, meaning any block can be accessed instantly, which enables both sequential and random file access47.

- **Block Mapping:** The file system is responsible for mapping logical file blocks to physical blocks on the disk and managing free space48.

### Layered File System

A layered approach divides file system functions into distinct modules:

1. **Logical File System:** Manages metadata, directories, and file protection49.

2. **File-Organization Module:** Maps logical file blocks to physical disk blocks50.

3. **Basic File System:** Reads and writes the physical blocks on the disk51.

4. **I/O Control Layer:** Handles the device drivers and low-level I/O52.

53535353

### Virtual File System (VFS)

VFS is an abstraction layer in the OS that provides a **common interface** for all file systems54. This separates generic file operations (like `open()`, `read()`) from their specific implementations55.

- **Advantage:** It allows the user to transparently access different types of file systems (e.g., a local disk, a remote network drive) without needing to know the underlying details56.

- It uses a `vnode` structure to represent any active file or directory consistently57.

[Image showing the VFS interface acting as a mediator between the general file-system interface and different file systems like 'local file system type 1', 'local file system type 2', and 'remote file system type 1'] 58

---

## 💾 Allocation Methods

Allocation methods determine how disk blocks are allocated to files59.

### 1. Contiguous Allocation

Each file occupies a **set of consecutive blocks** on the disk60606060.

- **Pros:** Excellent for sequential and direct access because the disk head can read the entire file with minimal movement61616161.

- **Cons:** Causes **external fragmentation**, where free space is broken into small, non-contiguous chunks. This can make it difficult to find space for new files, even if the total free space is sufficient62.

- **Example:** In the directory, a file `mail` might be listed with `start=19` and `length=6`. This means it occupies blocks 19, 20, 21, 22, 23, and 246363.

[Image showing contiguous allocation on a disk, with files 'count', 'f', 'tr', 'mail', and 'list' occupying continuous, sequential blocks] 64646464

### 2. Linked Allocation

Each file is a **linked list of disk blocks**, which can be scattered anywhere on the disk65. Each block contains a pointer to the next block in the file66.

- **Pros:** Solves external fragmentation, as any free block can be used67.

- **Cons:**
  
  - **Slow Direct Access:** To find the i-th block, you must traverse the list from the beginning68.
  
  - **Overhead:** Pointers take up space within each block.

- **Directory Entry:** Only needs to store the address of the first block69.

- **Example:** The directory shows `jeep` starts at block 9 and ends at block 25. Block 9 points to 16, 16 to 1, 1 to 10, 10 to 18, 18 to 22, and 22 points to 25, which is the end707070.

71

### 3. Indexed Allocation

This method uses a special block called an **index block** for each file, which contains pointers to all the file's data blocks72.

- **Pros:**
  
  - Supports **fast direct access**73.
  
  - Solves external fragmentation74.

- **Cons:** Wastes space if files are very small (an entire index block is still needed), and has overhead for the index block itself.

- **Directory Entry:** Contains the address of the index block75.

- **Example:** The directory entry for `jeep` points to index block 19. Block 19 contains a list of the data blocks for the file: 9, 16, 1, 10, 25, etc.767676767676767676767676767676767676767676.

77

---

## 🟩 Free Space Management

The file system must keep track of all free (unallocated) disk blocks to know where to store new files78787878.

### 1. Bit Vector (Bitmap)

A bit vector uses one bit to represent each block on the disk79.

- `0` = allocated block

- `1` = free block 80(Note: some systems use the reverse, 0=free, 1=allocated 81).

- **Example:** `0011110011...` means blocks 2, 3, 4, 5, 8, 9, etc., are free82.

- **Pros:** Simple and efficient for finding consecutive free blocks83.

### 2. Linked List

All free blocks are linked together in a list84. A pointer to the first free block is stored85.

- **Pros:** Easy to allocate a block (just take the first one off the list)868686.

- **Cons:** Not efficient for finding a *group* of contiguous blocks87.

### 3. Grouping / Counting

- **Grouping:** The first free block stores the addresses of `n` other free blocks. The last of these `n` blocks points to another group of free blocks88. This allows many free blocks to be found quickly89.

- **Counting:** Instead of linking individual blocks, the system stores the address of the first block in a contiguous free chunk and the *count* of how many free blocks follow it90. This is more efficient for large contiguous free spaces91.

---

## 💿 Disk Structure and Management

### Disk Structure (Logical vs. Physical View)

- **Physical View:** A disk is physically organized into **tracks** (concentric circles), **sectors** (subdivisions of a track), and **cylinders** (sets of tracks aligned vertically across platters)92.

- **Logical View:** Modern disks are managed as a one-dimensional array of **logical blocks**93939393. The file system uses these logical blocks for storage94. The disk controller complexly maps these logical blocks to the physical sectors, tracks, and cylinders95. This mapping is complex because of factors like defective sectors and zoned layouts (where outer tracks have more sectors)96.

### Formatting (Disk Initialization)

Before a disk can be used, it must be **formatted** by the OS97. This process prepares the disk by creating the tracks and sectors and setting up the necessary file system data structures, including the free-space management system98.

---

## ⏱️ Disk Head Scheduling

When multiple I/O requests are pending, the OS uses a scheduling algorithm to decide which request to service next99. This is done to reduce **seek time** (the time it takes the read/write head to move to the correct track) and improve performance100.

**Example Scenario:**

- **Request Queue:** 98, 183, 37, 122, 14, 124, 65, 67 101

- **Head Starts At:** 53 102

### 1. FCFS (First-Come, First-Served)

Services requests in the order they arrive103103103103.

- **Movement:** 53 → 98 → 183 → 37 → 122 → 14 → 124 → 65 → 67

- Result: Simple and fair, but very inefficient. It causes large, random swings of the disk head104. Total movement in this example is 640 cylinders105.
  
  106

### 2. SSTF (Shortest Seek Time First)

Selects the request that is **closest** to the current head position107107107107.

- **Movement:** 53 → 65 → 67 → 37 → 14 → 98 → 122 → 124 → 183

- **Result:** Much more efficient, reducing total movement to 236 cylinders108.

- Problem: Can cause starvation—requests far from the head might wait indefinitely if new, closer requests keep arriving109.
  
  110

### 3. SCAN (Elevator Algorithm)

The head moves in one direction (e.g., toward 0), servicing all requests in its path. When it reaches the end, it reverses direction111111111111.

- **Movement (assuming move toward 0 first):** 53 → 37 → 14 → 0 (reaches end) → 65 → 67 → 98 → 122 → 124 → 183

- Result: More balanced than SSTF, avoids starvation112.
  
  113

### 4. C-SCAN (Circular SCAN)

Similar to SCAN, but when the head reaches the end, it immediately **jumps back to the beginning** (e.g., to 0) *without* servicing requests on the return trip114. It then services requests in one direction only.

- **Movement:** 53 → 65 → 67 → 98 → 122 → 124 → 183 (reaches end) → (jumps to 0) → 14 → 37

- Result: Provides a more uniform and fair wait time than SCAN115.
  
  116

### 5. LOOK and C-LOOK

These are optimizations of SCAN and C-SCAN. The head only moves as far as the last request in each direction, rather than all the way to the end of the disk117117117117117117117117117. This reduces unnecessary head movement118.

119

### Selecting an Algorithm

- SSTF or LOOK are often good defaults120.

- SCAN and C-SCAN are better for systems with a heavy disk load to prevent starvation121.

- Performance also depends on the file allocation method; contiguous files (Contiguous Allocation) will naturally reduce head movement122.

---

## 🔄 Swap Management

**Swap space** is an area on the disk that acts as an extension of the computer's main memory (RAM)123.

- **Purpose:** When physical memory is low, the OS moves processes or, more commonly, individual **pages** from memory to the swap space on the disk. This is called **swapping**124.

- **Management:** The OS determines where the swap space is located on disk and how it is allocated to optimize the performance of the virtual memory system125. This is crucial because disk access is much slower than RAM access126.

---
