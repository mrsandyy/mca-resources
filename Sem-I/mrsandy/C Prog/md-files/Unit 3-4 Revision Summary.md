# Revision Summary: Advanced C & Web Technologies

## Unit 3: Advanced C Programming Concepts

### 1. C-Preprocessor Directives

The preprocessor is a text-substitution tool that processes code *before* compilation.

- **Compilation Pipeline:**
  
  1. **Preprocessor:** Resolves directives (`#`). Output: Expanded source.
  
  2. **Compiler:** Translates to Assembly. Checks syntax.
  
  3. **Assembler:** Converts Assembly to Machine Code (Object file).
  
  4. **Linker:** Combines object files/libraries into an Executable.

- **Key Directives:**
  
  - **Macros (`#define`):** * *Object-like:* Constants (e.g., `#define PI 3.14`).
    
    - *Function-like:* Code snippets (e.g., `#define SQUARE(x) ((x)*(x))`). *Note: Always use parentheses.*
  
  - **File Inclusion (`#include`):**
    
    - `<>`: Standard system directories.
    
    - `""`: Current directory first.
  
  - **Conditional Compilation:** (`#ifdef`, `#endif`, `#else`) used for cross-platform code or debugging.
  
  - **Misc:** `#undef` (remove macro), `#pragma` (compiler features like packing), `#error` (stop compilation).

### 2. User Defined Data Types

- **Structures (`struct`):**
  
  - Groups variables of **different types** under one name.
  
  - **Memory:** Sum of members + padding (for alignment).
  
  - Members have distinct addresses.

- **Unions (`union`):**
  
  - Members **share the same memory**.
  
  - **Memory:** Size of the **largest** member only.
  
  - Only one member can be active at a time; writing to one overwrites others.

- **Enumerations (`enum`):**
  
  - Set of named integer constants (e.g., `enum Color {RED, GREEN};`).
  
  - Improves readability over raw integers.

#### Structure vs. Union (Comparison)

| Feature               | Structure (`struct`)                              | Union (`union`)                                     |
| --------------------- | ------------------------------------------------- | --------------------------------------------------- |
| **Memory Allocation** | Allocates memory for **all members** summatively. | Allocates memory **only for the largest member**.   |
| **Total Size**        | Sum of all members + padding.                     | Size of the largest member only.                    |
| **Access**            | All members can be accessed simultaneously.       | Only **one member** can be accessed at a time.      |
| **Data Integrity**    | Changing one member has **no effect** on others.  | Changing one member **overwrites/corrupts** others. |
| **Use Case**          | Complex records (e.g., Student, Employee).        | Memory optimization, hardware registers.            |

### 3. Dynamic Memory Allocation (DMA)

Allocating memory at **runtime** on the **Heap** (vs. Stack for static vars).

- **`malloc(size)`:** Allocates raw bytes. Contains garbage values.

- **`calloc(n, size)`:** Allocates multiple blocks. Initializes to **zero**. Slightly slower.

- **`realloc(ptr, new_size)`:** Resizes existing allocated memory.

- **`free(ptr)`:** Releases memory. Failing to do so causes **Memory Leaks**.

- **Stack vs. Heap:** Stack is fast/auto-managed; Heap is flexible/manually-managed.

#### Static vs. Dynamic Memory Allocation

| Feature             | Static Memory                        | Dynamic Memory                            |
| ------------------- | ------------------------------------ | ----------------------------------------- |
| **Allocation Time** | **Compile Time** (Before execution). | **Runtime** (During execution).           |
| **Memory Segment**  | **Stack** (Auto-managed).            | **Heap** (Manually managed).              |
| **Size**            | Fixed size (must be known upfront).  | Flexible size (can change based on need). |
| **Deallocation**    | Automatic (when function ends).      | Manual (must use `free()`).               |

#### `malloc()` vs. `calloc()`

| Feature            | `malloc(size)`                 | `calloc(n, size)`                    |
| ------------------ | ------------------------------ | ------------------------------------ |
| **Full Name**      | Memory Allocation.             | Contiguous Allocation.               |
| **Arguments**      | 1 argument (total bytes).      | 2 arguments (item count, item size). |
| **Initialization** | Contains **Garbage Values**.   | Initializes all bytes to **Zero**.   |
| **Performance**    | Faster (no clearing overhead). | Slower (due to zero-initialization). |

### 4. File Handling

Persisting data to disk using `FILE *` streams.

- **Modes:**
  
  - `"r"`: Read (Error if missing).
  
  - `"w"`: Write (Overwrites/Creates).
  
  - `"a"`: Append (Adds to end).
  
  - `"rb"`, `"wb"`: Binary modes.

- **Operations:**
  
  - **Text:** `fprintf`, `fscanf`, `fgetc`, `fputc`.
  
  - **Binary:** `fwrite` (memory dump), `fread` (load to memory).

- **Random Access:**
  
  - `fseek()`: Move cursor (`SEEK_SET`, `SEEK_CUR`, `SEEK_END`).
  
  - `ftell()`: Get current position.
  
  - `rewind()`: Go to start.

## Unit 4: OOP & Web Technologies

### 1. Procedure Oriented (POP) vs. Object Oriented (OOP)

- **POP (e.g., C):**
  
  - Focus on **functions/procedures** (Top-Down).
  
  - Data is often global and insecure.
  
  - Harder to maintain/scale.

- **OOP (e.g., Java, C++):**
  
  - Focus on **Objects** (Bottom-Up).
  
  - Data is encapsulated (secure).
  
  - **Core Concepts:**
    
    1. **Class/Object:** Blueprint vs. Instance.
    
    2. **Encapsulation:** Hiding data, exposing methods.
    
    3. **Inheritance:** Reusing code (Parent -> Child).
    
    4. **Polymorphism:** Same interface, many forms (Overloading/Overriding).
    
    5. **Abstraction:** Hiding implementation details.

| Feature              | Procedure Oriented (POP)                             | Object Oriented (OOP)                                |
| -------------------- | ---------------------------------------------------- | ---------------------------------------------------- |
| **Primary Focus**    | **Functions/Algorithms** (Process-centric).          | **Objects/Data** (Data-centric).                     |
| **Program Division** | Divided into **Functions**.                          | Divided into **Classes/Objects**.                    |
| **Data Security**    | **Low**. Global data is accessible by all functions. | **High**. Data is **Encapsulated** (hidden/private). |
| **Design Approach**  | **Top-Down** (Main -> Sub-routines).                 | **Bottom-Up** (Components -> System).                |
| **Memory Mode**      | Static/Stack mostly.                                 | Dynamic/Heap heavily used.                           |
| **Reusability**      | Low (Copy-paste functions).                          | High (via **Inheritance**).                          |
| **Examples**         | C, Pascal, Fortran.                                  | Java, C++, Python.                                   |

### 2. Web Technologies Overview

- **HTML (Structure):** The skeleton. Defines *what* content is.

- **CSS (Presentation):** The style. Defines *how* it looks.

- **JS (Behavior):** The logic. Defines interactivity.

### 3. HTML (HyperText Markup Language)

- **DOM:** Tree structure representing the page.

- **Tags:** Semantic tags (`<header>`, `<footer>`) describe meaning.

- **Attributes:** Metadata (e.g., `src`, `href`, `id`, `class`).

### 4. CSS (Cascading Style Sheets)

- **Box Model:** Content → Padding → Border → Margin.

- **Selectors:** Class (`.name`), ID (`#name`), Element (`p`).

- **Integration:** External (`.css` file) is best practice over Inline or Internal.

### 5. JavaScript (JS)

- **Role:** Manipulates DOM, handles events, validates data.

- **Key Features:** * Variables (`let`, `const`).
  
  - Event-driven programming (e.g., `onclick`).
  
  - Accessing elements via `document.getElementById()`.

### 6. Data Transport & Storage

- **XML:**
  
  - Strict, verbose, tag-based.
  
  - Focus on data **structure**.
  
  - Self-descriptive but heavy.

- **JSON (JavaScript Object Notation):**
  
  - Lightweight, key-value pairs.
  
  - Standard for APIs.
  
  - Parsable by JS natively (`JSON.parse`, `JSON.stringify`).
  
  - Data Types: Strings, Numbers, Booleans, Arrays, Objects.

### 7. AJAX (Asynchronous JavaScript and XML)

- **Concept:** Update parts of a page **without reloading** the whole page.

- **Mechanism:**
  
  1. JS sends async request to server.
  
  2. User continues interacting (non-blocking).
  
  3. Server returns data (usually JSON).
  
  4. JS updates DOM.

- **Modern Implementation:** The `fetch()` API (uses Promises) replaces the older `XMLHttpRequest`.
