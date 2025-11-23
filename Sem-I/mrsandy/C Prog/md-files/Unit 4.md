# Unit 4: Object Oriented Programming & Web Technologies

## Table of Contents

1. [Procedure Oriented vs. Object Oriented Programming](https://www.google.com/search?q=%231-procedure-oriented-pop-vs-object-oriented-programming-oop "null")

2. [Introduction to Web Programming](https://www.google.com/search?q=%232-introduction-to-web-programming "null")

3. [HTML (HyperText Markup Language)](https://www.google.com/search?q=%233-html "null")

4. [CSS (Cascading Style Sheets)](https://www.google.com/search?q=%234-css "null")

5. [JavaScript](https://www.google.com/search?q=%235-javascript "null")

6. [XML (eXtensible Markup Language)](https://www.google.com/search?q=%236-xml "null")

7. [JSON (JavaScript Object Notation)](https://www.google.com/search?q=%237-json "null")

8. [AJAX (Asynchronous JavaScript and XML)](https://www.google.com/search?q=%238-ajax "null")

## 1. Procedure Programming vs. Object Oriented Programming (OOP)

### Concept Overview

**Procedure Oriented Programming (POP)** POP focuses fundamentally on the sequence of actions or "procedures" required to solve a problem. In this paradigm, the primary focus is on functions (algorithms) rather than the data itself. The program is conceptually viewed as a sequence of things to do, reading data, processing it, and writing it back.

- **Focus:** Focuses on the process or steps (algorithms) needed to solve a problem.

- **Structure (Top-Down):** The program is divided into small parts called functions or procedures.
  The design process starts with a high-level view of the overall system and breaks it down into smaller, manageable sub-routines or functions.

- **Data Handling:** Data typically moves freely around the system from function to function. Most functions share global data.
  Data is often global or passed openly between functions. This lack of restriction means any function can modify global data, leading to potential security risks and difficult-to-track bugs in large systems.

- **Drawback:** Difficult to maintain and scale. Data is less secure because it can be accessed/changed by any function.

- **Examples:** C, Pascal, COBOL, Fortran.

**Object Oriented Programming (OOP)** OOP shifts the perspective from "what needs to be done" to "what are the components involved." It focuses on modeling the software after real-world entities (Objects) containing both the data describing them and the logic to manipulate them.

- **Focus:** Focuses on the *objects* that users want to manipulate rather than the logic required to manipulate them.

- **Structure (Bottom-Up):** The design starts by identifying the smallest components (classes/objects) and composing them into larger systems.

- **Data Handling (Encapsulation):** Data is securely hidden inside objects. It can only be accessed or modified through public methods (functions) defined within that object, preventing accidental corruption by external parts of the program.

- **Advantage:** Modular, easier to maintain, reusable, and secure.

- **Examples:** Java, C++, Python, C#, Ruby.

### Visual Comparison

The diagram below illustrates how POP separates data and logic, creating a dependency web, whereas OOP encapsulates them into self-contained units.

```graphql
       POP ARCHITECTURE (Open Data)      OOP ARCHITECTURE (Encapsulated)
    +---------------------+          +-----------------------+
    |   Global Data       |          |       Object A        |
    +----------+----------+          | +-------------------+ |
               |                     | | Data (Private)    | |
      +--------v-------+             | +-------------------+ |
      | Function 1     |             | | Methods (Public)  | |
      +----------------+             | +---------+---------+ |
               |                     +-----------|-----------+
      +--------v-------+                         |
      | Function 2     |             +-----------v-----------+
      +----------------+             |       Object B        |
                                     | +-------------------+ |
                                     | | Data              | |
                                     | +-------------------+ |
                                     | | Methods           | |
                                     | +-------------------+ |
                                     +-----------------------+
```

### Detailed Comparison Table

| Feature             | Procedure Oriented (POP)                                                                                    | Object Oriented (OOP)                                                                                        |
| ------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| **Focus**           | Focuses on *doing* things (Algorithms/Procedures).                                                          | Focuses on the *things* themselves (Data/Objects).                                                           |
| **Division**        | Large programs are divided into functional units called **Functions**.                                      | Large programs are divided into entities called **Objects**.                                                 |
| **Data Security**   | **Low**. Data is often global and accessible by all functions, making it vulnerable to inadvertent changes. | **High**. Data is encapsulated (hidden) and access is controlled via public methods (Getters/Setters).       |
| **Design Approach** | **Top-Down Approach**. Starts with the main routine and breaks it down.                                     | **Bottom-Up Approach**. Starts with designing base classes and builds up to the main system.                 |
| **Memory Mode**     | Uses Static or Stack allocation mostly.                                                                     | Heavily relies on Dynamic (Heap) memory allocation for object creation.                                      |
| **Overloading**     | Not typically supported. You cannot have two functions with the same name.                                  | Supports **Polymorphism** (Function and Operator overloading), allowing the same name to behave differently. |
| **Reusability**     | Difficult to reuse code; usually requires copy-pasting functions.                                           | High reusability via **Inheritance**, allowing classes to inherit features from existing classes.            |

### Code Example Comparison

**POP (C Language Approach):** Notice that the `calculateArea` function is a standalone utility. It doesn't "belong" to the rectangle; it just accepts raw data. If `width` is changed accidentally elsewhere, the function won't know.

```c
struct Rectangle {
    int width;
    int height;
};

// Function definitions are separate from data
// The data structure acts only as a container
int calculateArea(struct Rectangle r) {
    return r.width * r.height;
}
```

**OOP (C++/Java Approach):** Here, the `Rectangle` is an active entity. The data (`width`, `height`) is `private`, meaning no outside code can mess with it directly. They *must* use `setDimensions`, which ensures the data is valid before setting it.

```cpp
class Rectangle {
    // Private access modifier ensures data hiding
    private:
        int width;
        int height;

    public:
        // Public interface to interact with the object
        void setDimensions(int w, int h) {
            if (w > 0 && h > 0) { // Logic validation (Encapsulation benefit)
                width = w;
                height = h;
            }
        }

        // Logic is inside the object; it knows its own data
        int calculateArea() {
            return width * height;
        }
};
```

### 1.2 Core OOP Concepts

#### 1. Class & Object

- **Class:** A blueprint or template. It defines the variables and methods common to all objects of a certain kind. It does not consume memory until an object is created.

- **Object:** An instance of a class. It is a real-world entity that has identity, state (data), and behavior (methods).

```cpp
// C++ Example Syntax
class Car {  // CLASS
    public:
    string brand;   // Attribute
    void honk() {   // Method
        cout << "Beep beep!";
    }
};

int main() {
    Car myCar;      // OBJECT
    myCar.brand = "Toyota";
    myCar.honk();
}
```

#### 2. Encapsulation

- **Definition:** Wrapping up data and methods into a single unit (class).

- **Purpose:** It prevents direct access to data by the outside world (Data Hiding). Access is controlled via public methods (Getters/Setters).

#### 3. Inheritance

- **Definition:** The mechanism where a new class (Child/Subclass) derives properties and characteristics from an existing class (Parent/Superclass).

- **Purpose:** Code Reusability.

#### 4. Polymorphism

- **Definition:** "Many forms". The ability of a message to be displayed in more than one form.
  
  - **Compile-time:** Function Overloading / Operator Overloading.
  
  - **Run-time:** Virtual functions (Overriding).

#### 5. Abstraction

- **Definition:** Hiding internal implementation details and showing only the essential features of the object.

- **Example:** You know how to drive a car (interface), but you don't need to know how the engine combustion works (implementation).

## 2. Introduction to Web Programming

Web programming encompasses the entire process of creating dynamic web applications. It involves a separation of concerns where structure, style, and logic are handled by distinct technologies. This separation allows developers to maintain code more easily and allows browsers to render pages efficiently.

**The Three Pillars of the Web:**

1. **HTML (Structure):** The skeleton of the page. It defines *what* exists (headers, paragraphs, images). Without HTML, there is no content.

2. **CSS (Presentation):** The skin and clothing. It defines *how* the content looks (colors, fonts, layout). Without CSS, the web would look like a plain Word document.

3. **JavaScript (Behavior):** The muscles and nervous system. It defines *what happens* when users interact (clicks, data loading, animations). It provides interactivity.

## 3. HTML

**HTML** (HyperText Markup Language) is the standard markup language for creating web pages. It is not a programming language (it has no logic or loops); it is a descriptive language used to define the structure of information.

### Key Concepts

- **Tags:** Keywords surrounded by angle brackets (e.g., `<html>`). They tell the browser how to render content.

- **Elements:** The complete unit: The start tag, the content inside, and the end tag (e.g., `<p>Hello</p>`).

- **Attributes:** Key-value pairs inside the start tag that provide metadata or configuration (e.g., `src="img.jpg"` tells the `img` tag *which* image to load).

- **DOM (Document Object Model):** When a browser loads HTML, it creates a tree-like structure of objects called the DOM. JavaScript uses this tree to find and modify HTML elements programmatically.

### Structure of an HTML Document

HTML5 introduced semantic tags (like `<header>`, `<footer>`, `<article>`) which describe the meaning of the content, not just its look.

```html
<!DOCTYPE html>              <!-- Declaration defining HTML5 standard -->
<html>                       <!-- The Root element wrapping everything -->
<head>                       <!-- Contains metadata, not visible on page -->
    <title>Page Title</title>
    <meta charset="UTF-8">   <!-- Character encoding -->
</head>
<body>                       <!-- The visible content area -->
    <header>
        <h1>Main Website Heading</h1>
    </header>

    <section>
        <p>This is a paragraph of text.</p>

        <!-- Image with Attribute (src = source, alt = alternative text) -->
        <img src="image.jpg" alt="A description for accessibility">

        <!-- Hyperlink (href = hypertext reference) -->
        <a href="[https://google.com](https://google.com)" target="_blank">Link to Google</a>
    </section>
</body>
</html>
```

## 4. CSS

**CSS** (Cascading Style Sheets) is responsible for the visual presentation. The term "Cascading" refers to the priority scheme that determines which style rule applies if more than one rule matches a specific element (e.g., the last rule defined often takes precedence).

### Syntax

A CSS rule-set consists of a selector and a declaration block.

```css
/* Selector */   /* Declaration Block */
   h1        {  color: blue; font-size: 12px;  }
                /* Property */  /* Value */
```

### The Box Model

Every element in web design is effectively a rectangular box. Understanding the layers of this box is critical for layout design. If you don't understand the box model, your layouts will often break or misalign.

```graphql
    +---------------------------------------+
    |               Margin                  |  <- Clears area outside the border (transparent)
    |  +---------------------------------+  |
    |  |            Border               |  |  <- A line going around the padding and content
    |  |  +---------------------------+  |  |
    |  |  |         Padding           |  |  |  <- Clears area around content (inside border)
    |  |  |  +---------------------+  |  |  |
    |  |  |  |      CONTENT        |  |  |  |  <- The actual text or image
    |  |  |  +---------------------+  |  |  |
    |  |  +---------------------------+  |  |
    |  +---------------------------------+  |
    +---------------------------------------+
```

### Implementation Methods

1. **Inline:** Applied directly to the tag. High specificity but poor maintainability.

2. **Internal:** Defined in the `<head>`. Good for single-page templates.

3. **External:** Defined in a separate `.css` file. **Best Practice** as it keeps content (HTML) and design (CSS) separate and cached by the browser.

### Code Example

```css
/* Class Selector (.): Applies to all elements with class="highlight" */
.highlight {
    background-color: yellow;
    font-weight: bold;
}

/* ID Selector (#): Applies to the ONE element with id="header" */
#header {
    text-align: center;
    padding: 20px; /* Space inside the border */
    margin-bottom: 10px; /* Space outside the border */
}

/* Element Selector: Applies to all <p> tags */
p {
    font-family: 'Arial', sans-serif;
    line-height: 1.6;
}
```

## 5. JavaScript

JavaScript (JS) is a high-level, interpreted (or JIT compiled) programming language. Unlike HTML/CSS, JS is a full logic language with loops, conditionals, and functions. It turns a static document into an interactive application.

### Key Capabilities

- **DOM Manipulation:** Adding, removing, or changing HTML elements on the fly.

- **Attribute Modification:** Changing where an image points or where a link goes dynamically.

- **Style Manipulation:** changing CSS properties based on user events (e.g., Dark Mode).

- **Data Validation:** Checking if an email address is valid before sending it to the server.

### Syntax Basics

Modern JavaScript (ES6+) introduced features like `let` and `const` for safer variable declarations compared to the older `var`.

```js
// Variables
let count = 10;          // Mutable variable (can change)
const API_KEY = "XYZ";   // Immutable constant (cannot change)

// Functions
function greet(name) {
    // Template literals (backticks) allow easy string insertion
    return `Hello, ${name}!`;
}

// DOM Manipulation
// This looks for an element with id="demo" and changes its internal text and style
function changeText() {
    const element = document.getElementById("demo");
    element.innerHTML = "<strong>Text Updated!</strong>"; // Parses HTML tags inside
    element.style.color = "red"; // Direct CSS manipulation
    element.style.backgroundColor = "#f0f0f0";
}
```

### Integration (Event-Driven)

JavaScript usually runs in response to **Events** (clicks, mouse movements, key presses).

```html
<!-- The onclick attribute connects the User Action to the Logic -->
<button onclick="changeText()">Click Me</button>

<p id="demo">Original Text</p>

<script>
    // Scripts are usually placed at the end of the body to ensuring HTML loads first
    console.log("Page Loaded");
</script>
```

## 6. XML

**XML** (eXtensible Markup Language) is a markup language strictly designed for **data storage and transport**.

- **Purpose:** Unlike HTML, which focuses on *display*, XML focuses on *structure* and *meaning*.

- **Self-Descriptive:** The tags explain the data. `<price>100</price>` clearly indicates the value is a price.

- **Extensibility:** You define your own tags. There are no pre-defined tags like `<h1>` or `<p>`.

### XML vs. HTML Summary

| XML                                                | HTML                                                         |
| -------------------------------------------------- | ------------------------------------------------------------ |
| **Transport:** Designed to carry data.             | **Display:** Designed to show data.                          |
| **Strict:** Case sensitive, requires closing tags. | **Loose:** Not case sensitive, somewhat forgiving of errors. |
| **Meta:** Describes what data *is*.                | **Visual:** Describes what data *looks like*.                |
| **Whitespace:** Preserves whitespace exactly.      | **Whitespace:** Collapses multiple spaces into one.          |

### Syntax Example

XML requires a root element that wraps all other elements.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<library> <!-- Root Element -->
    <book id="101">
        <title>The Great Gatsby</title>
        <author>F. Scott Fitzgerald</author>
        <price currency="USD">15.00</price>
        <genre>Fiction</genre>
    </book>
    <book id="102">
        <title>1984</title>
        <author>George Orwell</author>
        <price currency="USD">12.00</price>
    </book>
</library>
```

## 7. Introduction to JSON

**JSON** (JavaScript Object Notation) is a lightweight text-based format for storing and transporting data. While it is derived from JavaScript syntax, it is language-independent and can be read by Python, Java, C++, etc.

- **Why JSON over XML?** JSON is less verbose (no closing tags like `</title>`), resulting in smaller file sizes and faster network transmission. It is also natively understood by JavaScript, making it the standard for web APIs.

- **Data Types:** JSON supports Strings, Numbers, Booleans, Arrays, Objects, and Null.

### Syntax Rules

1. Data consists of name/value pairs (`"key": value`).

2. Data is separated by commas.

3. Curly braces `{}` hold objects.

4. Square brackets `[]` hold arrays (ordered lists).

5. **Crucial:** Keys *must* be strings enclosed in double quotes.

### JSON Example Structure

This example shows an object containing an array of objects, a common pattern in API responses.

```json
{
  "company": "TechCorp",
  "founded": 2010,
  "active": true,
  "offices": ["New York", "London", "Tokyo"],
  "employees": [
    { "id": 1, "name": "John Doe", "role": "Developer" },
    { "id": 2, "name": "Anna Smith", "role": "Designer" }
  ]
}
```

### Processing JSON in JavaScript

Since JSON is text, we must convert it to a JS object to use it, and back to text to send it.

```js
// 1. Parsing (String -> Object)
// Simulating data received from a server
let serverResponse = '{"name":"John", "age":30}';
let userObj = JSON.parse(serverResponse); 
console.log(userObj.name); // Output: John

// 2. Stringifying (Object -> String)
// Preparing data to send to a server
let data = { name: "Alice", city: "NY" };
let payload = JSON.stringify(data);
// 'payload' is now strictly formatted text ready for transmission
```

## 8. AJAX

**AJAX** stands for **A**synchronous **J**avaScript **A**nd **X**ML.

- **Concept:** AJAX allows web pages to update asynchronously by exchanging data with a web server behind the scenes.

- **The User Experience:** In a traditional model, if you clicked "Next Page," the entire screen would go white and reload. With AJAX, only the specific content area updates while the rest of the page (header, sidebar, music player) remains static and interactive.

- **Note on Name:** Although it has "XML" in the name, modern AJAX implementations almost exclusively use **JSON** for data transport.

### How AJAX Works (Diagram)

The key is the separation of the User Interface (UI) thread from the Network Request.

```graphql
    Browser (Client)                   Server
       |                                  |
       | 1. User Clicks Button            |
       | (JS creates Request object)      |
       |--------------------------------->|
       | 2. Send Async Request            |
       | (Browser continues working)      |
       |                                  | 3. Server Processes Request
       |                                  | (Queries Database, etc.)
       |                                  | 4. Creates Response (JSON)
       |                                  |
       | 5. Return Response               |
       |<---------------------------------|
       |                                  |
       | 6. JS Callback function triggers |
       | 7. DOM is updated with new data  |
       |    (No Page Reload)              |
       |                                  |
```

### The `XMLHttpRequest` Object (Legacy)

This was the original way to perform AJAX. It is verbose and based on event handlers.

- **Steps:**
  
  1. Create `new XMLHttpRequest()`.
  
  2. Assign a function to `onreadystatechange` to handle the answer.
  
  3. Call `open('GET', 'url')`.
  
  4. Call `send()`.

### The `Fetch` API (Modern Standard)

Modern development uses `fetch()`, which is cleaner and uses **Promises** to handle asynchronous success or failure.

**Syntax Example (Fetch):**

```js
const url = "[https://api.example.com/users/1](https://api.example.com/users/1)";

// fetch() returns a Promise
fetch(url)
  .then(response => {
    // Step 1: Check if the network connection was successful
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    // Step 2: Parse the incoming stream as JSON
    return response.json(); 
  })
  .then(data => {
    // Step 3: Use the actual data to update the UI
    console.log(data);
    document.getElementById("username").innerText = data.name;
    document.getElementById("status").innerText = "Active";
  })
  .catch(error => {
    // Step 4: Handle any errors (network down, 404, etc.)
    console.error('Fetch operation failed:', error);
    document.getElementById("error-msg").innerText = "Failed to load user.";
  });
```

### Summary of Web Technologies Flow

To build a modern feature (like a Comment section):

1. **HTML** builds the text area and "Post" button.

2. **CSS** styles the button to look clickable and the text area to be readable.

3. **JS** listens for the click event on the button.

4. **AJAX** takes the comment text and sends it to the server *without* reloading the page.

5. **JSON** is the format the comment travels in (e.g., `{ "text": "Great post!", "author": "Me" }`).

6. The server saves it and returns a "Success" JSON.

7. **JS** receives the success and inserts the new comment HTML into the list instantly.
