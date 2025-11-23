Here are your detailed study notes for Unit-2, covering Arrays, Pointers, and Strings in C.

---

## 1. Array

An **array** is a fixed-size, sequential collection of elements of the **same data type**. It's a way to store multiple values under a single name. The elements are stored in **contiguous** (one after another) memory locations.

### Declaration and Initialization of Array

#### **Declaration**

To declare an array, you need to specify the data type of its elements and its size.

**Syntax**: `data_type array_name[size];`

- `data_type`: The type of elements the array will hold (e.g., `int`, `float`, `char`).

- `array_name`: The name of the array (an identifier).

- `size`: The maximum number of elements the array can hold.

Example:

int marks[100]; // Declares an array named 'marks' that can hold 100 integers.

Memory for this array would be allocated contiguously. If an `int` takes 4 bytes, this array would occupy `100 * 4 = 400` consecutive bytes in memory.

#### **Initialization**

You can initialize an array at the time of declaration.

1. **Initializing all elements**:
   
   C
   
   ```
   int numbers[5] = {10, 20, 30, 40, 50};
   ```

2. **Partial initialization**: If you provide fewer elements than the size, the remaining elements are automatically initialized to **zero**.
   
   C
   
   ```
   int numbers[5] = {10, 20}; // numbers will be {10, 20, 0, 0, 0}
   ```

3. **Initializing without size**: The compiler automatically determines the size based on the number of elements.
   
   C
   
   ```
   int numbers[] = {1, 2, 3, 4}; // Compiler sets the size to 4
   ```

### Traversing and Manipulation in Arrays

Elements in an array are accessed using an **index**, which starts from **0** and goes up to `size - 1`.

#### **Traversing**

Traversing means visiting each element of the array exactly once. This is typically done using a `for` loop.

C

```
#include <stdio.h>

int main() {
    int numbers[5] = {10, 20, 30, 40, 50};
    int i;

    // Traversing the array to print its elements
    printf("Array elements are: ");
    for (i = 0; i < 5; i++) {
        printf("%d ", numbers[i]); // Accessing element at index i
    }
    printf("\n");

    return 0;
}
// Output: Array elements are: 10 20 30 40 50
```

#### **Manipulation**

This involves changing the data in the array, such as inserting, deleting, or updating elements.

- Updating: Simply assign a new value at a specific index.
  
  numbers[2] = 99; // The array becomes {10, 20, 99, 40, 50}

- **Insertion/Deletion**: These operations are more complex because they require shifting other elements to maintain the contiguous nature of the array. For an MCA syllabus, understanding the concept is key. For example, to insert an element at index `k`, you must shift all elements from `k` onwards one position to the right.

---

### Searching and Sorting in Array

#### **Searching**

Searching is the process of finding the location (index) of a specific element in an array.

1. **Linear Search**:
   
   - **Method**: Sequentially checks each element of the array for the target value until a match is found or the entire array has been searched.
   
   - **Best for**: Unsorted arrays.
   
   - **Time Complexity**: $O(n)$, because in the worst case, you have to check every element.

  C

```
int linearSearch(int arr[], int size, int key) {
    for (int i = 0; i < size; i++) {
        if (arr[i] == key) {
            return i; // Return the index if found
        }
    }
    return -1; // Return -1 if not found
}
```

2. **Binary Search**:
   
   - **Prerequisite**: The array **must be sorted**.
   
   - **Method**: It follows a "divide and conquer" approach. It repeatedly divides the search interval in half. If the value of the search key is less than the item in the middle of the interval, it narrows the interval to the lower half. Otherwise, it narrows it to the upper half.
   
   - **Time Complexity**: $O(\log n)$, as it halves the search space with each comparison, making it very efficient for large arrays.

  C

```
int binarySearch(int arr[], int size, int key) {
    int low = 0, high = size - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] == key) {
            return mid; // Found
        }
        if (arr[mid] < key) {
            low = mid + 1; // Search in the right half
        } else {
            high = mid - 1; // Search in the left half
        }
    }
    return -1; // Not found
}
```

#### **Sorting**

Sorting is the process of arranging array elements in a specific order (ascending or descending).

1. **Bubble Sort**:
   
   - **Method**: It repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order. The pass through the list is repeated until the list is sorted. The largest elements "bubble" to the end.
   
   - **Time Complexity**: $O(n^2)$. It's simple but inefficient for large datasets.

  C

```
void bubbleSort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap arr[j] and arr[j+1]
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}
```

2. **Selection Sort**:
   
   - **Method**: The algorithm divides the array into two parts: a sorted sub-array and an unsorted sub-array. It repeatedly finds the minimum element from the unsorted part and moves it to the end of the sorted part.
   
   - **Time Complexity**: $O(n^2)$.

  C

```
void selectionSort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int min_idx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }
        // Swap the found minimum element with the first element
        int temp = arr[min_idx];
        arr[min_idx] = arr[i];
        arr[i] = temp;
    }
}
```

---

### Passing Array to Function

In C, when you pass an array to a function, you are actually passing the **address of its first element**. This means the function works directly on the original array, not a copy. This is a form of **pass-by-reference**.

- Any changes made to the array inside the function **will affect** the original array in the calling function.

- Because only the address is passed, the function doesn't know the array's size. Therefore, you should always pass the size as a separate argument.

Function Declaration Syntax:

void functionName(int arr[], int size);

or

void functionName(int *arr, int size); // Both are equivalent

**Example**:

C

```
#include <stdio.h>

// This function modifies the array
void modifyArray(int data[], int size) {
    for (int i = 0; i < size; i++) {
        data[i] = data[i] * 2; // Double each element
    }
}

int main() {
    int my_arr[] = {1, 2, 3, 4, 5};
    int size = 5;

    modifyArray(my_arr, size);

    printf("Modified array: ");
    for (int i = 0; i < size; i++) {
        printf("%d ", my_arr[i]);
    }
    printf("\n");

    return 0;
}
// Output: Modified array: 2 4 6 8 10
```

---

## 2. Pointers

A **pointer** is a special variable that stores the **memory address** of another variable. Instead of holding a value (like an `int` or `char`), it "points to" the location where a value is stored. 🧠

### Declarations and Initialization

Declaration:

data_type *pointer_name;

The * tells the compiler that pointer_name is a pointer. data_type is the type of the variable that the pointer will point to. This is important for pointer arithmetic.

**Example**: `int *ptr;` // ptr is a pointer that can store the address of an integer variable.

**Key Pointer Operators**:

- `&` (**Address-of Operator**): Returns the memory address of a variable.

- `*` (**Dereference Operator** or Value-at-Address Operator): Accesses the value stored at the address held by the pointer.

Initialization:

A pointer is initialized by assigning it the address of a variable.

C

```
int age = 25;  // A normal integer variable
int *p_age;    // A pointer to an integer

p_age = &age;  // Initialize p_age with the address of 'age'
```

**Using the Dereference Operator**:

C

```
printf("Address of age: %p\n", p_age);   // Prints the memory address
printf("Value of age: %d\n", *p_age);    // Prints the value at that address, which is 25
```

A NULL pointer is a pointer that does not point to any memory location. It's good practice to initialize pointers to NULL if they don't have a valid address to point to yet.

int *ptr = NULL;

---

### Pointer Arithmetic

Arithmetic operations on pointers are different from those on regular numbers. When you perform arithmetic, the pointer moves by a number of memory locations determined by its base data type.

If `ptr` is an `int` pointer (`sizeof(int)` is 4 bytes):

- `ptr++` or `ptr + 1`: Increments the address stored in `ptr` by 4 bytes, making it point to the next integer.

- `ptr--` or `ptr - 1`: Decrements the address by 4 bytes.

- `ptr + n`: Points `n * sizeof(int)` bytes ahead.

- `ptr1 - ptr2`: If `ptr1` and `ptr2` are pointers to the same array, this gives the number of elements between them.

**This is why pointers and arrays are so closely related.**

---

### Pointers and Arrays

In C, an array's name is a constant pointer to its first element.

This means:

- `arr` is equivalent to `&arr[0]`.

- `arr[i]` is equivalent to `*(arr + i)`.

This relationship allows you to use pointer syntax to access array elements.

C

```
#include <stdio.h>

int main() {
    int arr[5] = {10, 20, 30, 40, 50};
    int *p = arr; // p now points to the first element of arr

    printf("First element (using array syntax): %d\n", arr[0]);
    printf("First element (using pointer): %d\n\n", *p);

    printf("Third element (using array syntax): %d\n", arr[2]);
    printf("Third element (using pointer): %d\n", *(p + 2));

    return 0;
}
```

---

### Array of Pointers

An **array of pointers** is an array where each element is a pointer variable. Each pointer in the array can point to a different memory location.

**Declaration**: `data_type *array_name[size];`

**Example**: `int *ptr_arr[3];` // An array of 3 integer pointers.

This is commonly used to store an array of strings. Since a string is a sequence of characters, you can have an array of `char` pointers, where each pointer points to the first character of a string.

C

```
#include <stdio.h>

int main() {
    char *names[3] = {
        "Alice",
        "Bob",
        "Charlie"
    };

    printf("Names are:\n");
    for (int i = 0; i < 3; i++) {
        printf("%s\n", names[i]);
    }
    return 0;
}
```

---

## 3. Strings in C

In C, a **string** is not a fundamental data type. It is implemented as a **one-dimensional array of characters** that is terminated by a special character called the **null character (`\0`)**. This null character is crucial as it marks the end of the string.

**Declaration and Initialization**:

C

```
// Method 1: Using a string literal (null character is added automatically)
char greeting[] = "Hello";

// Method 2: Initializing as a character array (must add null character manually)
char greeting2[] = {'H', 'e', 'l', 'l', 'o', '\0'};
```

In memory, `greeting` would look like: `H | e | l | l | o | \0`

### Operations on Strings

Basic operations involve reading, writing, and manipulating strings using loops.

- **Reading a string**: `scanf("%s", str);` (stops at whitespace) or `gets(str);` (reads until newline, but is unsafe and can cause buffer overflows).

- **Writing a string**: `printf("%s", str);`

### String Library Functions

To perform complex operations easily, C provides a rich set of string-handling functions in the `<string.h>` header file.

| **Function**                   | **Description**                                                                                                                                 | **Example**                                                    |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| **`strlen(str)`**              | Calculates the length of the string `str` (excluding the `\0`).                                                                                 | `int len = strlen("Hello"); // len is 5`                       |
| **`strcpy(dest, src)`**        | Copies the string `src` into the string `dest`.                                                                                                 | `char dest[10]; strcpy(dest, "Hi");`                           |
| **`strcat(dest, src)`**        | Appends (concatenates) the string `src` to the end of `dest`.                                                                                   | `char d[20]="Hello "; strcat(d, "World");`                     |
| **`strcmp(str1, str2)`**       | Compares `str1` and `str2` lexicographically (like in a dictionary). Returns: `0` if equal, `< 0` if `str1` < `str2`, `> 0` if `str1` > `str2`. | `if (strcmp(s1, s2) == 0) printf("Equal");`                    |
| **`strchr(str, ch)`**          | Returns a pointer to the first occurrence of the character `ch` in the string `str`, or `NULL` if not found.                                    | `char *p = strchr("hello", 'l'); // p points to the first 'l'` |
| **`strstr(haystack, needle)`** | Returns a pointer to the first occurrence of the substring `needle` in the string `haystack`, or `NULL` if not found.                           | `char *p = strstr("computer", "put");`                         |





Of course! Here are your detailed study notes on Insertion Sort and Memory Allocation Schemes in C, prepared for your MCA exam.

---

## 1. Insertion Sort

Insertion sort is a simple sorting algorithm that builds the final sorted array one item at a time. It's much like how you would sort a hand of playing cards. You keep the cards you've already sorted in order, and then you take a new card and insert it into its correct position among the sorted ones. 🃏

### How It Works

The algorithm divides the array into two conceptual parts: a **sorted sub-array** and an **unsorted sub-array**. Initially, the sorted sub-array contains only the first element. The algorithm then iterates through the unsorted part, picking one element at a time (the `key`) and inserting it into its correct position in the sorted part.

**Algorithm Steps:**

1. Start from the second element (index 1), as the first element is already a "sorted" sub-array of size one.

2. Select the current element. Let's call it `key`.

3. Compare `key` with the elements in the sorted sub-array, moving from right to left.

4. If an element in the sorted part is greater than `key`, shift it one position to the right to make space.

5. Continue this process until you find an element that is smaller than or equal to `key`, or you reach the beginning of the array.

6. Insert `key` into the newly created space.

7. Repeat steps 2-6 for all elements in the unsorted part.

---

### C Code Implementation

C

```
#include <stdio.h>

void insertionSort(int arr[], int n) {
    int i, key, j;
    // Start from the second element (i=1)
    for (i = 1; i < n; i++) {
        // Select the element to be inserted
        key = arr[i];
        j = i - 1;

        /* Move elements of arr[0..i-1], that are           greater than key, to one position ahead           of their current position */
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        // Place the key at its correct position
        arr[j + 1] = key;
    }
}

void printArray(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
}

int main() {
    int arr[] = {12, 11, 13, 5, 6};
    int n = sizeof(arr) / sizeof(arr[0]);

    printf("Unsorted array: \n");
    printArray(arr, n);

    insertionSort(arr, n);

    printf("Sorted array: \n");
    printArray(arr, n);

    return 0;
}
```

---

### Complexity and Characteristics

- **Time Complexity**:
  
  - **Worst Case**: $O(n^2)$ — This occurs when the array is sorted in reverse order.
  
  - **Average Case**: $O(n^2)$ — Occurs when the elements are in a random order.
  
  - **Best Case**: $O(n)$ — This occurs when the array is already sorted. The inner loop never executes.

- **Space Complexity**: $O(1)$ — It's an **in-place** sorting algorithm, meaning it requires only a constant amount of extra memory space.

**Key Characteristics**:

- **Efficient for small datasets**: It outperforms more complex algorithms like Merge Sort or Quick Sort for small `n`.

- **Adaptive**: Its performance improves if the array is already "partially sorted."

- **Stable**: It does not change the relative order of elements with equal keys.

- **In-place**: Requires no additional storage space.

- **Online**: It can sort a list as it receives it.

---

## 2. Memory Allocation Schemes in C

In C, memory allocation is the process of reserving a portion of computer memory for a program to use. This can happen either at compile-time or at run-time. The memory assigned to a C program is typically divided into four main segments.

1. **Code Segment**: Contains the compiled machine code of the program.

2. **Data Segment**: Stores global, static, and constant variables.

3. **Stack**: Used for static memory allocation. Stores local variables, function parameters, and return addresses. It follows a LIFO (Last-In, First-Out) principle.

4. **Heap**: A pool of memory used for dynamic memory allocation. It's a large, unstructured region available for the program to use at run-time.

There are two primary memory allocation schemes in C:

---

### Static Memory Allocation

In **static memory allocation**, the size and location of memory are determined at **compile-time**, before the program is executed. The memory is allocated on the **stack** (for local variables) or in the **data segment** (for global/static variables).

- **Mechanism**: The allocation and deallocation are managed automatically by the compiler. When a function is called, memory for its local variables is allocated on the stack. When the function returns, that memory is automatically freed.

- **Example**:
  
  C
  
  ```
  void myFunction() {
      int buffer[100]; // 100 * sizeof(int) bytes allocated on the stack
      // ... use buffer ...
  } // 'buffer' is automatically deallocated when the function ends.
  ```

- **Pros**:
  
  - **Fast**: Allocation is very quick, typically just moving the stack pointer.
  
  - **Automatic**: Memory management is handled by the compiler, reducing the risk of errors like memory leaks.

- **Cons**:
  
  - **Inflexible**: The memory size must be a constant known at compile-time. You cannot create an array whose size is determined by user input.
  
  - **Wastage**: Memory is allocated for the maximum possible size, which might be more than what is needed, leading to wastage.
  
  - **Stack Overflow**: If you try to allocate a very large block of memory on the stack, it can exceed the stack's size, causing a stack overflow error.

---

### Dynamic Memory Allocation

In **dynamic memory allocation**, memory is allocated at **run-time** from the **heap**. This provides flexibility, as the program can request memory as needed during its execution. The programmer has full control and responsibility for managing this memory.

This is achieved using functions from the standard library header **`<stdlib.h>`**.

#### Key Functions

1. **`malloc()` (memory allocation)**
   
   - `void* malloc(size_t size);`
   
   - Allocates a single block of memory of `size` bytes.
   
   - The memory is **not initialized**; it contains garbage values.
   
   - Returns a `void*` pointer to the first byte of the allocated block. You must cast this pointer to the appropriate type.
   
   - Returns `NULL` if the allocation fails.

2. **`calloc()` (contiguous allocation)**
   
   - `void* calloc(size_t num_elements, size_t element_size);`
   
   - Allocates memory for an array of `num_elements`, each of `element_size` bytes.
   
   - The allocated memory is **initialized to zero**.
   
   - Also returns a `void*` pointer or `NULL` on failure.

3. **`free()`**
   
   - `void free(void* ptr);`
   
   - Deallocates a block of memory that was previously allocated by `malloc`, `calloc`, or `realloc`.
   
   - **Crucial for preventing memory leaks**. A **memory leak** occurs when you allocate memory but forget to free it, making it unusable for the rest of the program's lifetime.

4. **`realloc()` (re-allocation)**
   
   - `void* realloc(void* ptr, size_t new_size);`
   
   - Changes the size of a previously allocated memory block pointed to by `ptr`.
   
   - It may move the memory block to a new location if necessary.

#### Example of Dynamic Allocation

C

```
#include <stdio.h>
#include <stdlib.h>

int main() {
    int n, i;
    int *arr; // Pointer to store the base address of the block

    printf("Enter the number of elements: ");
    scanf("%d", &n);

    // Dynamically allocate memory for 'n' integers using malloc
    arr = (int*) malloc(n * sizeof(int));

    // Always check if malloc was successful
    if (arr == NULL) {
        printf("Memory allocation failed!\n");
        return 1; // Exit with an error code
    }

    // Use the allocated memory
    printf("Enter %d numbers:\n", n);
    for (i = 0; i < n; i++) {
        scanf("%d", &arr[i]);
    }

    printf("You entered: ");
    for (i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    // IMPORTANT: Free the allocated memory when done
    free(arr);

    return 0;
}
```

- **Pros**:
  
  - **Flexible**: Memory size can be decided at run-time.
  
  - **Efficient Use of Memory**: Allocate only what you need.

- **Cons**:
  
  - **Slower**: Allocation takes more time than static allocation.
  
  - **Programmer's Responsibility**: You must manually `free()` the memory. Forgetting to do so causes **memory leaks**. Using a pointer after it has been freed (a **dangling pointer**) leads to undefined behavior.

---

### Static vs. Dynamic Memory Allocation: A Summary

| **Feature**         | **Static Memory Allocation**      | **Dynamic Memory Allocation**             |
| ------------------- | --------------------------------- | ----------------------------------------- |
| **Allocation Time** | Compile-time                      | Run-time                                  |
| **Memory Area**     | Stack & Data Segment              | Heap                                      |
| **Management**      | Automatic (by compiler)           | Manual (by programmer)                    |
| **Speed**           | Faster                            | Slower                                    |
| **Flexibility**     | Inflexible (size is fixed)        | Highly flexible (size can vary)           |
| **Lifespan**        | Tied to the scope of the variable | Exists until explicitly freed by `free()` |
| **Example**         | `int arr[10];`                    | `int *arr = malloc(10 * sizeof(int));`    |
