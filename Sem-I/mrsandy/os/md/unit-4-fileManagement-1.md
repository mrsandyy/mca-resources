# 1. Introduction to Files

#### Definition of a File

A file is a **named collection of related information** that is stored on secondary storage (like a disk)2. From a user's perspective, it's the smallest logical unit for storing data3. The operating system provides a logical view of data, hiding the physical storage details4.

#### File Attributes

An operating system records specific properties for each file, known as attributes5. These include:

- **Identifier:** A unique system-generated tag (not the name) that identifies the file within the file system6.

- **Type:** Specifies the file's format (e.g., text, executable, source code)7.

- **Location:** A pointer to the file's exact position on the storage device8.

- **Size:** The current size of the file9.

- **Protection:** Access-control information that defines who can read, write, or execute the file10.

- **Time, Date, and User Identification:** Records of creation, last modification, and last access for security and tracking11.

#### Common File Types

File types help the OS and users understand how a file should be used, often indicated by an extension12.

| **File Type**   | **Extensions**          | **Description / Purpose**                       |
| --------------- | ----------------------- | ----------------------------------------------- |
| **Executable**  | `.exe`, `.com`, `.bin`  | Ready-to-run machine language programs13.       |
| **Object**      | `.obj`, `.o`            | Compiled machine code that is not yet linked14. |
| **Source Code** | `.c`, `.java`, `.asm`   | Source programs written in various languages15. |
| **Batch**       | `.bat`, `.sh`           | Command scripts for the command interpreter16.  |
| **Text**        | `.txt`, `.doc`          | Plain text or document files17.                 |
| **Archive**     | `.zip`, `.tar`          | Related files grouped and often compressed18.   |
| **Multimedia**  | `.mpeg`, `.mp3`, `.avi` | Audio or audio/video (A/V) files19.             |

---

### 2. File Access Methods

This defines how data is read from and written to a file20.

#### Sequential Access

This is the simplest access method. Information in the file is processed **in order, one record after another**21. A file pointer tracks the current read/write position and moves automatically22. This follows a tape-like model23.

#### Direct Access

Also known as random access, this method allows records or blocks to be accessed **directly in any order** using their block number24. This is essential for applications like databases, where you need to access a specific piece of data (e.g., flight 713's info) without reading the entire file25.

#### Other Access Methods (Indexed Access)

This method is built on top of direct access and uses an **index** that stores pointers to data blocks26. To find a specific record, the system first searches the index for the record's pointer, and then uses that pointer to access the data block directly27. This is much faster than searching the entire file.

---

### 3. Directory and File System Structure

#### Directory Structure

Storage devices are often divided into **partitions** or **volumes**28. Each volume contains a file system and has a **directory** that stores details about the files within it, such as their name, type, size, and location29.

#### File System Organization

File systems are often built in layers to separate different levels of functionality30.

- **Layered File System:**
  
  - **Logical File System:** Manages metadata, directories, and file protection31. This is what the user's application interacts with.
  
  - **File-Organization Module:** Maps logical blocks to physical blocks on the disk32.
  
  - **Basic File System:** Reads and writes physical blocks on the disk33.
  
  - **I/O Control Layer:** Handles device drivers and I/O operations34.

![Image of a layered file system diagram](https://encrypted-tbn3.gstatic.com/licensed-image?q=tbn:ANd9GcRN2TFn2vpL81GpmMauoIXvYSbNG9Ekbkkm2mNRuzakQ448g4MtQuVe3AVTk3z8ljuJKKw-R5rMvEvPsnPLckDdLepYf22gctqL740Mo0ZGFlgprPY)

Shutterstock

Explore

- Virtual File System (VFS):
  
  VFS is an abstraction layer within the OS that provides a common interface for different file systems35. This allows the OS to support multiple types of file systems (e.g., local disk, remote network file system) transparently. The VFS separates generic file operations from their specific implementations36.

---

### 4. Allocation Methods

This describes how disk blocks are allocated to files.

#### Contiguous Allocation

Each file occupies a **set of consecutive blocks** on the disk37373737.

- **Pros:** Fast sequential and direct access, as the disk head can move minimally38383838.

- **Cons:** Suffers from **external fragmentation**39. It can be difficult to find a large enough contiguous chunk of space for a new file.

- **Example:** The directory stores the `start` block and `length` of the file. A file "mail" starting at block 19 with length 6 would occupy blocks 19, 20, 21, 22, 23, and 2440.

#### Linked Allocation

Each file is a **linked list of disk blocks**, which can be scattered anywhere on the disk41414141. Each block contains a pointer to the next block in the file42.

- **Pros:** No external fragmentation43. Files can grow dynamically.

- **Cons:** **Slow direct access**, as you must traverse the list from the beginning to find the i-th block44.

- **Example:** The directory entry for file "jeep" points to the start block (9). Block 9 points to block 16, block 16 points to block 1, and so on, until the last block (25)45454545.

#### Indexed Allocation

This method uses an **index block** (or "inode") for each file. The index block is a special block that contains pointers to *all* the data blocks of the file464646.

- **Pros:** Supports efficient direct access47. No external fragmentation48.

- **Cons:** Wastes space for the index block itself49.

- **Example:** The directory entry for "jeep" points to its index block (19). Block 19 contains the list of data blocks for the file: 9, 16, 1, 10, 25 50505050.

---

### 5. Free-Space Management

The OS must keep track of all free (unallocated) disk blocks. Common methods include:

1. Bit Vector (Bitmap):
   
   Each block on the disk is represented by one bit in a vector. For example, 0 = free and 1 = allocated51. Finding a free block involves searching the vector for the first 0 bit52.

2. Linked List:
   
   The free blocks are linked together. A pointer to the first free block is stored53. Allocating a block just means taking the first block off the list.

3. **Grouping / Counting:**
   
   - **Grouping:** The first free block stores the addresses of `n` other free blocks. This allows many free blocks to be found quickly54.
   
   - **Counting:** Stores the starting block address and a *count* of contiguous free blocks, which is more efficient than listing each block individually55.

---

### 6. Secondary Storage Management

#### Disk Structure (Logical vs. Physical View)

- **Physical View:** A disk is physically organized into **tracks** (concentric circles), **sectors** (subdivisions of a track), and **cylinders** (sets of tracks aligned vertically across platters) 56.

- **Logical View:** Modern disks are managed as a one-dimensional (1D) array of **logical blocks**5757. The file system uses these logical blocks, and the disk controller maps them to physical (track, sector) addresses58585858.

#### Disk Head Scheduling

The OS must decide the order in which to service disk I/O requests to reduce seek time (the time it takes to move the disk head) 59.

- FCFS (First-Come, First-Served):
  
  Services requests in the order they arrive. Simple and fair, but very inefficient60606060.

- SSTF (Shortest Seek Time First):
  
  Selects the request that is closest to the current head position61. It's more efficient than FCFS but can cause starvation (requests far away may never be serviced)62.

- SCAN (Elevator Algorithm):
  
  The head moves in one direction (e.g., towards cylinder 0), servicing all requests in its path. When it reaches the end, it reverses direction63636363. This prevents starvation.

- C-SCAN (Circular SCAN):
  
  Similar to SCAN, but when the head reaches the end, it immediately jumps back to the beginning (cylinder 0) without servicing requests on the return trip64646464. This provides a more uniform wait time.

- LOOK / C-LOOK:
  
  These are variants of SCAN/C-SCAN where the head only moves as far as the last request in that direction, not to the physical end of the disk65656565. This reduces unnecessary head movement66.

![Image of LOOK disk scheduling](https://encrypted-tbn1.gstatic.com/licensed-image?q=tbn:ANd9GcSFKZsz8ENnAqdU6yP5X0s9IZM-FlLPOpWERziRSCfjg2DFC2GfTQEtTBwZkssZr2xX_Dp39eKWT9z_mP0lUB-73Fw0fqA92H3FVciUmyC77f6Yzjk)

Shutterstock

#### Disk Formatting

Before a new disk can be used, it must be formatted. This process prepares the disk by creating tracks, sectors, and setting up the file system data structures (like the free-space manager)67.

#### Swap Management

Swap space is a designated area on the disk that acts as an extension of main memory68. Swapping is the process of moving pages or entire processes between RAM and this swap space when physical memory is low69. The OS manages the location and allocation of this space to optimize performance for the virtual memory system70. Swap I/O is typically faster than standard file system I/O because it is allocated in large, contiguous chunks 71.
