# Unit 1. Introduction to Problem Solving Methods

Problem-solving in computing involves designing a step-by-step procedure to solve a specific problem. This procedure is often represented as an algorithm or a flowchart before being translated into a programming language.

---

### Algorithms and Flowcharts

#### **Algorithm**

An **algorithm** is a finite, well-defined sequence of steps or instructions to solve a particular problem. It's a blueprint for the code.

**Characteristics of a Good Algorithm:**

- **Input**: It must have zero or more well-defined inputs.

- **Output**: It must produce at least one well-defined output.

- **Finiteness**: It must terminate after a finite number of steps.

- **Definiteness**: Each step must be clear, precise, and unambiguous.

- **Effectiveness**: Each step must be basic enough to be carried out, in principle, by a person using only pen and paper.

**Example: Algorithm to find the sum of two numbers.**

1. **START**

2. **DECLARE** three integer variables: `num1`, `num2`, `sum`.

3. **READ** the values for `num1` and `num2`.

4. **CALCULATE** `sum = num1 + num2`.

5. **PRINT** the value of `sum`.

6. **STOP**

#### **Flowchart**

A **flowchart** is a graphical or pictorial representation of an algorithm. It uses standard symbols to show the sequence of operations and the flow of data.

**Common Flowchart Symbols:**

| **Symbol** | **Name**         | **Description**                                                                                                |
| ---------- | ---------------- | -------------------------------------------------------------------------------------------------------------- |
|            | **Terminator**   | Represents the start or end point of the program (e.g., START, STOP).                                          |
|            | **Input/Output** | Represents an input operation (READ) or an output operation (PRINT).                                           |
|            | **Process**      | Represents a calculation or an operation (e.g., `sum = a + b`).                                                |
|            | **Decision**     | Represents a point where a decision is made (e.g., `is a > b?`). It has two exit paths: Yes/True and No/False. |
|            | **Connector**    | Used to connect different parts of a flowchart.                                                                |
|            | **Flow Lines**   | Arrows that indicate the direction and flow of logic.                                                          |

**Example: Flowchart to find the sum of two numbers.**

---

### Top-Down and Bottom-Up Approaches

These are two different strategies for program design and problem-solving.

#### **Top-Down Approach (Stepwise Refinement)**

In this approach, a complex problem is broken down into smaller, more manageable sub-problems. Each sub-problem is then solved independently. The process continues until each sub-problem is simple enough to be implemented directly. This is the core idea behind **procedural programming**.

- **Process**: Main module -> Sub-modules -> Sub-sub-modules.

- **Focus**: Breaking down the problem.

- **Example**: Designing a university management system. You start with the main system, then break it down into modules like 'Student Admission', 'Faculty Management', and 'Examination'. Each of these is further broken down into smaller functions.

#### **Bottom-Up Approach**

In this approach, you start by designing and implementing the most basic or low-level modules first. These modules are then combined and integrated to form higher-level modules. This process continues until the complete system is built. This is often used in **object-oriented programming**.

- **Process**: Basic modules -> Integrated modules -> Complete system.

- **Focus**: Integrating and building up from components.

- **Example**: Building a C library. You first create fundamental functions like `printf`, `scanf`, etc. Then you can use these basic functions to build more complex library functions.

| **Feature**        | **Top-Down Approach**                                            | **Bottom-Up Approach**                                             |
| ------------------ | ---------------------------------------------------------------- | ------------------------------------------------------------------ |
| **Focus**          | Breaking down a problem into sub-problems.                       | Building a solution from basic components.                         |
| **Main Challenge** | Defining the sub-problems correctly.                             | Integrating the components seamlessly.                             |
| **Communication**  | Less communication needed between modules initially.             | High communication needed between modules from the start.          |
| **Testing**        | Starts from the top module. Stubs (dummy modules) may be needed. | Starts from the bottom modules. Drivers (test modules) are needed. |
| **Used In**        | Procedural Programming (like C).                                 | Object-Oriented Programming.                                       |

---

## 2. 'C' Language Fundamentals

### Compilation Process in C

The compilation process transforms your source code (a `.c` file) into an executable file that the computer can run. This process has four main stages.

**Diagram of the Compilation Process:**

1. **Preprocessing**: The preprocessor (`cpp`) takes the source code as input. It handles directives that start with `#`, such as:
   
   - `#include`: Includes header files into the code.
   
   - `#define`: Substitutes macros.
   
   - `#if`, `#endif`: Handles conditional compilation.
   
   - It also removes comments.
   
   - **Output**: An expanded source code file (e.g., with a `.i` extension).

2. **Compilation**: The compiler takes the expanded source code (`.i` file) and translates it into assembly code, which is specific to the target processor architecture. The compiler checks the code for syntax and semantic errors.
   
   - **Output**: An assembly code file (e.g., with a `.s` extension).

3. **Assembly**: The assembler takes the assembly code (`.s` file) and converts it into machine-readable object code. This code is in binary format but is not yet executable. It contains placeholders for calls to functions that are defined elsewhere (e.g., in libraries).
   
   - **Output**: An object file (e.g., with a `.o` or `.obj` extension).

4. **Linking**: The linker takes one or more object files and links them with necessary library files to produce a single executable file. It resolves the addresses of functions called in the code (like `printf()`) by finding their definitions in the libraries and placing them into the final file.
   
   - **Output**: An executable file (e.g., `a.out` on Linux, `.exe` on Windows).

---

### Basic Constructs of C

These are the fundamental building blocks of a C program.

- **C Character Set**: The set of valid characters in C, including:
  
  - **Letters**: A-Z, a-z
  
  - **Digits**: 0-9
  
  - **Special Characters**: `~ ! @ # % ^ & * ( ) _ - + = { } [ ] | \ : ; " ' < > , . ? /`
  
  - **White Spaces**: Space, newline, tab.

- **Keywords**: Reserved words that have a special meaning to the compiler and cannot be used as variable names. There are 32 keywords in standard C.
  
  - Examples: `int`, `float`, `if`, `else`, `for`, `while`, `return`, `struct`, `sizeof`.

- **Identifiers**: Names given to entities like variables, functions, and arrays.
  
  - **Rules for naming identifiers**:
    
    - Must start with a letter (a-z, A-Z) or an underscore (`_`).
    
    - Can be followed by letters, digits, or underscores.
    
    - Cannot be a keyword.
    
    - C is case-sensitive (`age` and `Age` are different).
    
    - **Valid**: `_value`, `total_sum`, `var1`.
    
    - **Invalid**: `1var` (starts with a digit), `total-sum` (contains hyphen), `int` (is a keyword).

- Variables: A named memory location used to store data. Its value can be changed during program execution.
  
  data_type variable_name;
  
  int age = 25;

- **Data Types**: Specifies the type of data a variable can hold.
  
  - **Primary Data Types**:
    
    - `int`: For integers (e.g., 10, -50).
    
    - `char`: For single characters (e.g., 'A', 'b').
    
    - `float`: For single-precision floating-point numbers (e.g., 3.14).
    
    - `double`: For double-precision floating-point numbers (more precision than `float`).
    
    - `void`: Represents the absence of a type.

- **Constants**: Values that do not change during program execution.
  
  - **Literal Constants**: `100` (integer), `3.14` (float), `'A'` (character).
  
  - **Symbolic Constants**:
    
    - Using `#define`: `#define PI 3.14159`
    
    - Using `const` keyword: `const float PI = 3.14159;`

---

### Storage Classes

A storage class defines the **scope** (visibility), **lifetime** (duration), and **initial value** of a variable or function within a C program.

1. **`auto` (Automatic)**
   
   - **Scope**: Block scope (only visible within the block `{...}` where it is declared).
   
   - **Lifetime**: Exists only as long as the block is being executed.
   
   - **Storage**: Stored in the stack memory.
   
   - **Initial Value**: Garbage value (unpredictable).
   
   - **Note**: This is the default storage class for all local variables.

  

```c
void myFunction() {
    auto int count = 10; // 'auto' keyword is optional here
    printf("%d", count);
} // 'count' is destroyed here
```

2. **`extern` (External)**
   
   - **Purpose**: To declare a global variable that is defined in another file. It tells the compiler that the variable exists somewhere else.
   
   - **Scope**: Global (accessible throughout the program, across multiple files).
   
   - **Lifetime**: Exists as long as the program is running.
   
   - **Storage**: Stored in the data segment.
   
   - **Initial Value**: Zero, by default.

  

```c
// file1.c
int global_var = 100; // Definition

// file2.c
#include <stdio.h>
extern int global_var; // Declaration
void display() {
    printf("Value: %d", global_var); // Accesses the variable from file1.c
}
```

3. **`static` (Static)**
   
   - **Purpose**: It has two main uses depending on where it's declared.
   
   - **Inside a function (Local Static Variable)**:
     
     - **Scope**: Block scope.
     
     - **Lifetime**: Persists between function calls. It is initialized only once.
     
     - **Initial Value**: Zero, by default.

  

```c
void counter() {
    static int count = 0; // Initialized only once
    count++;
    printf("%d ", count);
}
int main() {
    counter(); // Prints 1
    counter(); // Prints 2
    counter(); // Prints 3
    return 0;
}
```

- **Outside a function (Global Static Variable)**:
  
  - The scope of the variable is limited to the file in which it is declared. It cannot be accessed from other files using `extern`.
4. **`register` (Register)**
   
   - **Purpose**: A hint to the compiler to store the variable in a fast CPU register instead of RAM.
   
   - **Scope**: Block scope.
   
   - **Lifetime**: Exists only within the block.
   
   - **Initial Value**: Garbage value.
   
   - **Note**: This is only a request. The compiler can ignore it if no registers are free. You cannot get the address of a register variable (`&var` is not allowed).

  

```c
void fastLoop() {
    register int i;
    for (i = 0; i < 10000; i++) {
        // Faster access to 'i'
    }
}
```

---

### Operators

Operators are symbols that perform operations on variables and values (operands).

| **Category**              | **Operators**                    | **Description**                                                                                                                                             |
| ------------------------- | -------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Arithmetic**            | `+`, `-`, `*`, `/`, `%`          | Perform mathematical calculations. `%` is the modulus operator (remainder).                                                                                 |
| **Relational**            | `==`, `!=`, `>`, `<`, `>=`, `<=` | Compare two operands and return a boolean result (1 for true, 0 for false).                                                                                 |
| **Logical**               | `&&`, `\|`, `!`                  | `&&` (AND), `\|` (OR), `!` (NOT). Used to combine or negate conditions.                                                                                     |
| **Bitwise**               | `&`, `\|`, `^`, `~`, `<<`, `>>`  | Perform operations on individual bits of data.                                                                                                              |
| **Assignment**            | `=`, `+=`, `-=`, `*=`, `/=`      | Assign values to variables. `a += 5` is shorthand for `a = a + 5`.                                                                                          |
| **Increment/Decrement**   | `++`, `--`                       | Increase or decrease the value by 1. **Prefix (`++a`)**: increments first, then uses the value. **Postfix (`a++`)**: uses the value first, then increments. |
| **Conditional (Ternary)** | `? :`                            | A shorthand for an `if-else` statement. `(condition) ? value_if_true : value_if_false;`                                                                     |
| **Special**               | `sizeof()`, `&`, `*`, `,`        | `sizeof()` returns the size of a variable. `&` is the address-of operator. `*` is the pointer dereference operator. `,` is the comma operator.              |

**Operator Precedence and Associativity** determines the order in which operators are evaluated in an expression. For example, `*` and `/` have higher precedence than `+` and `-`.

---

### Control Structures

Control structures dictate the flow of execution in a program.

#### **Branching (Decision Making)**

1. **`if` statement**: Executes a block of code if a condition is true.
   
   ```c
   if (score > 90) {
      printf("Grade A");
   }
   ```

2. **`if-else` statement**: Executes one block of code if the condition is true, and another if it is false.
   
   ```c
   if (age >= 18) {
      printf("Eligible to vote.");
   } else {
      printf("Not eligible to vote.");
   }
   ```

3. **`if-else-if` Ladder**: Used for multi-path decisions.
   
   ```c
   if (score >= 90) {
      printf("Grade A");
   } else if (score >= 80) {
      printf("Grade B");
   } else {
      printf("Grade C");
   }
   ```

4. **`switch` statement**: An alternative to the `if-else-if` ladder, used to test a variable against a list of values (`case`).
   
   - The `break` statement is crucial to exit the `switch` block after a match is found.
   
   - The `default` case is executed if no other case matches.

```c
char grade = 'B';
switch (grade) {
    case 'A':
        printf("Excellent!");
        break;
    case 'B':
        printf("Well done");
        break;
    default:
        printf("Invalid Grade");
}
```

#### **Looping (Iteration)**

1. **`while` loop**: An **entry-controlled** loop. The condition is checked *before* executing the loop body. The loop runs as long as the condition is true.
   
   ```c
   int i = 1;
   while (i <= 5) {
      printf("%d ", i);
      i++;
   } // Output: 1 2 3 4 5
   ```

2. **`do-while` loop**: An **exit-controlled** loop. The condition is checked *after* executing the loop body. This guarantees the loop will run at least once.
   
   ```c
   int i = 1;
   do {
      printf("%d ", i);
      i++;
   } while (i <= 5); // Output: 1 2 3 4 5
   ```

3. **`for` loop**: Used when the number of iterations is known. It combines initialization, condition checking, and update into one line.
   
   ```c
   int i;
   for (i = 1; i <= 5; i++) {
      printf("%d ", i);
   } // Output: 1 2 3 4 5
   ```

---

## 3. Functions in C

### Library Functions

These are built-in, pre-defined functions in C that are grouped into header files. To use them, you must include the appropriate header file using `#include`.

**Common Header Files and Functions:**

- **`<stdio.h>`** (Standard Input/Output)
  
  - `printf()`: Prints formatted output to the console.
  
  - `scanf()`: Reads formatted input from the console.
  
  - `fopen()`, `fclose()`: For file operations.

- **`<string.h>`** (String Handling)
  
  - `strlen()`: Returns the length of a string.
  
  - `strcpy()`: Copies one string to another.
  
  - `strcmp()`: Compares two strings.

- **`<math.h>`** (Mathematical Functions)
  
  - `sqrt()`: Calculates the square root.
  
  - `pow()`: Calculates the power of a number.
  
  - `sin()`, `cos()`: Trigonometric functions.

- **`<stdlib.h>`** (Standard Library)
  
  - `malloc()`, `calloc()`: For dynamic memory allocation.
  
  - `exit()`: Terminates program execution.
  
  - `rand()`: Generates a random number.

---

### User-Defined Functions

Functions are self-contained blocks of code that perform a specific task. They make code modular, reusable, and easier to manage.

A function has three main aspects:

1. Function Declaration (Prototype): Informs the compiler about the function's name, return type, and parameters. It's usually placed at the top of the file.
   
   return_type function_name(type param1, type param2, ...);
   
   int add(int a, int b);

2. **Function Definition**: Contains the actual code that executes when the function is called.
   
   ```c
   int add(int a, int b) { // Function header
      // Function body
      int sum = a + b;
      return sum; // Returns a value
   }
   ```

3. Function Call: Invokes the function to execute its code.
   
   int result = add(10, 20); // Calling the function

#### **Parameter Passing Mechanisms**

1. **Call by Value**:
   
   - A copy of the actual argument's value is passed to the function.
   
   - Changes made to the formal parameter inside the function **do not** affect the original actual argument.

```c
void increment(int x) {
    x = x + 1; // Changes only the local copy 'x'
}
int main() {
    int num = 10;
    increment(num);
    printf("%d", num); // Output: 10 (num is unchanged)
    return 0;
}
```

2. **Call by Reference**:
   
   - The address of the actual argument is passed to the function (using pointers).
   
   - The function can access and modify the original value at that address.
   
   - Changes made inside the function **do** affect the original actual argument.

```c
void swap(int *x, int *y) { // Receives addresses
    int temp = *x;
    *x = *y;
    *y = temp;
}
int main() {
    int a = 10, b = 20;
    swap(&a, &b); // Passes addresses of a and b
    printf("a = %d, b = %d", a, b); // Output: a = 20, b = 10
    return 0;
}
```

---

### Recursion

Recursion is a process in which a function calls itself, either directly or indirectly. A recursive function must have two key components:

1. **Base Case**: A condition that stops the recursion. Without a base case, the function would call itself indefinitely, leading to a stack overflow error.

2. **Recursive Step**: The part of the function where it calls itself, typically with modified arguments that move it closer to the base case.

**Example: Factorial of a number using recursion**

The factorial of n (n!) is the product of all positive integers up to n.

n! = n * (n-1) * (n-2) * ... * 1

n! = n * (n-1)!

```c
#include <stdio.h>

long factorial(int n) {
    // Base Case: Factorial of 0 is 1
    if (n == 0) {
        return 1;
    }
    // Recursive Step: n * factorial of (n-1)
    else {
        return n * factorial(n - 1);
    }
}

int main() {
    int num = 5;
    printf("Factorial of %d is %ld", num, factorial(num)); // Output: 120
    return 0;
}
```

**How `factorial(3)` works:**

1. `factorial(3)` is called. It returns `3 * factorial(2)`.

2. `factorial(2)` is called. It returns `2 * factorial(1)`.

3. `factorial(1)` is called. It returns `1 * factorial(0)`.

4. `factorial(0)` is called. It hits the base case and returns `1`.

5. The calls then resolve back up the chain:
   
   - `factorial(1)` returns `1 * 1 = 1`.
   
   - `factorial(2)` returns `2 * 1 = 2`.
   
   - `factorial(3)` returns `3 * 2 = 6`.

**Recursion vs. Iteration**

| **Feature**     | **Recursion**                                                                                               | **Iteration (Loops)**                                                     |
| --------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| **Logic**       | Simpler, more elegant code for problems that are naturally recursive (e.g., tree traversals).               | Can be more complex to write and understand.                              |
| **Performance** | Slower due to the overhead of function calls and stack management.                                          | Faster as it avoids function call overhead.                               |
| **Memory**      | Uses more memory because each function call adds a new frame to the call stack. Can lead to stack overflow. | Uses less memory; variables are typically stored in a single stack frame. |
| **Termination** | Terminates when the base case is reached.                                                                   | Terminates when the loop condition becomes false.                         |
