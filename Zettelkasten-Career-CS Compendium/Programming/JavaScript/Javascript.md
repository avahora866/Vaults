---
tags:
  - "#TRUNK"
---
JavaScript is a interpreted (and JIT-compiled) language. JavaScript requires an Engine to execute code.

The most famous engine is Google's V8, written in C++. It is used in Chrome, Edge and crucially Node.js. Other browsers also have have their engines (e.g. `SpiderMonkey` for Firefox, `JavaScriptCode` for Safari)

The Engine does two primary things:
- Memory Heap: Where memory allocation happens (variables, objects)
- Call Stack; Where stack frames are executed (your code logic)

# The Host Environment
The engine implements the ECMAScript Standard (the language syntax), but it does not provide the APIs for web programming (`console.log`, `windows` etc).

The engine runs inside a Host Environment. The Host injects extra capabilities (APIs) into the global scope for JavaScript to use.

- When you run JS in Chrome, the environment is **V8 + Web APIs**. The browser injects global variables into the engine that allow you to interact with the webpage.
	- **The DOM:** `document.getElementById(...)` (Allows modifying HTML).
	- **The Window:** `window.innerWidth` (Allows reading browser size).
	- **Interaction:** `alert()`, `prompt()`.
- Node.js is, quite literally, the V8 Engine ripped out of the Chrome browser and installed on a server. However, the creators of Node **removed** the Web APIs (since a server has no screen or window) and **replaced** them with C++ bindings to the Operating System.
	- **File System:** `fs.readFile(...)` (Allows reading files from the hard drive).
	- **OS:** `os.cpus()` (Allows checking server CPU status).
	- **Network:** `http.createServer(...)` (Allows listening to ports).

Because these environments are different, the "root" object that holds all these tools changes.
- In the **Browser**, the top-level global object is `window`.
- In **Node.js**, there is no window. The top-level global object is called `global`.

# Browser
The browser is a container that holds distinct tools that work in a sequence:
1. The Networking Component: This handles HTTP requests, DNS lookup, and TCP/IP. It talks to the outside world
2. The Rendering Engine (e.g., Blink): This parses HTML and CSS. Its job is to draw visual boxes on the screen
3. The JavaScript Engine (e.g. V8): This is a calculator. It parses and executes JavaScript code. It has no ability to make network requests on its own

## Step by step
1. The Requests (Networking): User types `google.com` and hits enter
	- The Networking Component takes over. It sends a HTTP GET request to the server
	- The JavaScript Engine is currently inactive. It is doing nothing
2. The Response (HTML): Server sends back an HTML file (text)
3. The Parsing (Rendering Engine): The Rendering Engine takes that text and starts reading it top-to-bottom to build the page structure
	- It sees `<div>`, it draws a box
	- It sees `<h1>`, it draws a headline 
4. The Handoff (The Critical Moment): The Rendering Engine encounters a line of text that looks like this `<script src="logic.js"></script>`
	- The Rendering Engine pauses. It cannot understand this file. It hands the file over to the JavaScript Engine
5. Execution (JavaScript Engine): The JavaScript Engine reads the code in `logic.js` and executes it line by line
	- If the code says `console.log(1+1)`, the engine calculates 2
	- If the code needs to fetch more data, the engine asks the browser's networking component to do it 

# Memory and Data
JavaScript uses the stack and heap memory structure:
- Stack memory (Static Allocation): Linear data structure that operations on LIFO principle. 
	- The stack is used for primitive values (numbers, strings, Booleans) because their size is fixed and known at compile time. 
	- It is also where function calls and their local variables are managed (the call stack)
	- Access is fast because of its structure
- Heap memory (Dynamic Allocation): 
	- Large unstructured region of memory. 
	- Used for non-primitive values (objects, arrays, and functions) because their size is dynamic and can change at run
	- Variables stored on the stack that hold non-primitive values actually store a reference (or address) that points to the objects location in the heap
	- Access is generally slower than stack

## Variables
In JavaScript variables are containers, but the type is associated with the value, not the variable. This is Dynamic Typing

- `var`: does not respect the block scopes. It leaks out to the entire function (or global scope)
- `let` and `const`: these are the solution to `var` and are block scoped 

# Loose typing and implicit coercion
In JavaScript, the Engine tries to be "helpful." It will dynamically convert types to make the code run rather than crash.
```javascript
int x = 5 + "5"
```
No compiler error
## Truthy and Falsy
In JavaScript, **everything** has an inherent boolean value. We call these "Truthy" or "Falsy."

There are exactly **6 Falsy values** in JavaScript. If you put these in an `if (...)`, they resolve to `false`:
1. `false`
2. `0` (The number zero)
3. `""` (Empty string)
4. `null`
5. `undefined`
6. `NaN` (Not a Number)

**Everything else is Truthy.**
- `[]` (Empty array) -> **True** (This trips up many Java devs who expect empty lists to be false).
- `{}` (Empty object) -> **True**.
- `"0"` (String zero) -> **True**.

## Equality
- **`==` (Loose Equality):** Allows type coercion.
    - `5 == "5"` is **True**. (The engine converts the string "5" to a number, then compares).
    - `0 == false` is **True**.
    - `null == undefined` is **True**.
- **`===` (Strict Equality):** No coercion. Checks Value AND Type.
    - `5 === "5"` is **False**.
    - `0 === false` is **False**.

**The Academic Rule:** Always use `===` (Triple Equals). Pretend `==` does not exist unless you have a very specific reason to check for both null and undefined simultaneously.

# Functions & The Execution Context
In JavaScript, **functions are "First-Class Citizens."**
This means a function is treated exactly like any other variable (like an `int` or `String`).
1. You can pass a function as an argument to another function.
2. You can return a function from a function.
3. You can assign a function to a variable.

## Function Declaration vs. Expression

This distinction affects **Hoisting** (the order in which the Engine reads code).

**A. Function Declaration (Hoisted):** The Engine scans the file _before_ executing it. It finds these functions and puts them in memory first. You can call them _before_ you write them.

```javascript
// This works!
sayHello(); 

function sayHello() {
    console.log("Hello");
}
```

**B. Function Expression (Not Hoisted):** Here, the function is anonymous and assigned to a `const` variable. Variables (let/const) are not initialized until the line is reached.

```javascript
// This crashes! ReferenceError
sayHi(); 

const sayHi = function() {
    console.log("Hi");
};
```

## The Execution Context (The Stack Frame)

Every time a function runs, it creates a new **Execution Context**. This is similar to a Stack Frame in Java, but it contains a special "invisible" record called the **Lexical Environment**.

**Lexical Scoping:** In JavaScript, "Scope" is determined by **where the function sits physically in the code**, not where it is called.

Consider this "Closure" example. 
```javascript
function outer() {
    const secret = "I am hidden";
    
    function inner() {
        // inner sits INSIDE outer physically.
        // Therefore, it has access to 'secret', even after outer() finishes.
        console.log(secret);
    }
    
    return inner;
}

const myFunc = outer(); // outer runs and returns 'inner'. outer is now popped off the stack.
myFunc(); // We are calling 'inner' later.
```
**In Java (traditionally):** Once `outer()` finishes, its local variables are garbage collected. `secret` should be gone. **In JavaScript:** Because `inner` was defined _inside_ `outer`, it keeps a "backpack" (Closure) containing `secret`. It remembers the environment where it was created.

### Engine
The Engine reads your file **twice**.
**Pass 1: The Memory Allocation Phase (Parsing)** The Engine scans the code _before_ running it. It looks for variable and function declarations to set aside memory space.
1. **Function Declarations (`function xyz() {}`)**: The Engine sees the `function` keyword. It grabs the **entire function** (name + body) and puts it in memory immediately.
    - _Result:_ You can use it before it is defined.
2. **Variables (`const`, `let`)**: The Engine sees the variable name. It reserves a spot in memory but marks it as **"Uninitialized"**.
    - _Result:_ If you try to use it before the line where it is defined, the Engine throws an error (ReferenceError).
**Pass 2: The Execution Phase** The Engine runs the code line by line.
**Summary:**
- You choose **Hoisting** by writing `function name() {...}`.
- You choose **Non-Hoisting** by writing `const name = function() {...}`.

## Closures deep dive
JavaScript historically didn't have classes. It used **Functions** to hold state. **Closure is simply the mechanism that allows a function to hold state.**
```javascript
function createCounter() {
    // 1. Variable declared in Local Scope
    let count = 0;
    
    // 2. We define a new function INSIDE
    function increment() {
        count++; // It references the variable above
        console.log(count);
    }
    
    // 3. We return the function definition
    return increment;
}

// 4. We store the result in a variable
const myCounter = createCounter();
```

1. **Line 4 runs:** We call `createCounter`. A new Stack Frame (Execution Context) is created.
2. Inside that frame, `count` is set to `0`.
3. The function `increment` is defined. **Crucial Step:** When `increment` is born, it looks around and sees `count`. It attaches a hidden link to `count`.
    - Think of this link as a **Backpack**. The function `increment` puts `count` inside its backpack.
4. `createCounter` finishes and returns `increment`.
5. **The Stack Frame for `createCounter` is destroyed.** In a normal language, `count` would be garbage collected here.
6. **HOWEVER:** Because `increment` is still alive (stored in `myCounter`), and because `increment` has `count` in its backpack, **`count` is preserved in the Heap.**
	1. We returned the increment function this means that whoever calls the resulting increment function will not error as increment has a hidden link to the count variable

# The Browser Environment
To the **Browser**, that HTML is not just text. It is parsed into a live data structure called the **DOM (Document Object Model)**.

## The DOM is a Tree Data Structure
When the browser loads your HTML, it creates a massive object graph in memory.
- The root is `document`.
- `<html>` is a child node.
- `<body>` is a child of `html`.
- `<div>` is a child of `body`.

This is **exactly** like parsing an XML file or a JSON object in Java. It is a tree of nodes.

## The API Connection
The Browser injects APIs. The `document` object is the primary API. It serves as the bridge between the **V8 Engine** (Logic) and the **Rendering Engine** (Pixels).

When you write `document.getElementById(...)`, you are using JavaScript to reach across the bridge and modify the C++ objects in the Rendering Engine.

## Selection and Manipulation
In the old days (jQuery era), this was messy. In modern JavaScript, we primarily use two powerful methods to find nodes in this tree:

- **`document.querySelector('css-selector')`**: Finds the **first** match.
- **`document.querySelectorAll('css-selector')`**: Finds **all** matches (returns a NodeList).

```javascript
    // HTML: <h1 id="title" class="header">Hello World</h1>

// Select it
const titleNode = document.querySelector('#title');

// Modify it (Mutation)
titleNode.textContent = "Hello Professor"; // Changes text
titleNode.style.color = "red";             // Changes CSS
```

# Asynchronous JavaScript (The Event Loop)
**In JavaScript (Browser & Node):** JavaScript is **Single-Threaded**. There is only **one** thread executing code. If you block it (e.g., waiting 2 seconds for a database), the entire browser freezes. You cannot click buttons, scroll, or close the tab. The interface dies.

To solve this, JavaScript uses the **Event Loop**.

## The Call Stack vs. The Callback Queue
We have two main data structures:
1. **The Stack:** Where synchronous code runs (LIFO - Last In, First Out).
2. **The Queue:** Where asynchronous tasks wait to be processed (FIFO - First In, First Out).

When you run a slow operation (like fetching data), the Engine does **not** wait.
1. It offloads the task to the **Web API** (Browser) or **C++ Bindings** (Node).
2. It immediately moves to the next line of code.
3. When the data comes back later, the callback function is placed in the **Queue**.
4. **The Event Loop** checks: "Is the Stack empty? Yes? Okay, move the first item from the Queue to the Stack."

```javascript
console.log("1");

setTimeout(function() {
    console.log("2");
}, 0); // Note: The delay is 0 milliseconds

console.log("3");
```

The order of console logs above is 1, 3, 2.
When the engine gets to the `setTimeout` line it offloads the task to the web API. Once it completes the code after, it comes back and then the `setTimeout` is placed on the queue. If stack is empty it moves the function in queue to call stack and executes the function

## Escaping Callback Hell
In the old days, if you wanted to do three async things in a row (`Login -> Get User Data -> Get Recent Posts`), you had to nest callbacks inside callbacks. We called this "Callback Hell."

```javascript
// The Old Way (The Pyramid of Doom)
login(function(user) {
    getUserData(user, function(data) {
        getPosts(data, function(posts) {
            console.log(posts);
        });
    });
});
```

To fix this, JavaScript introduced **Promises** (ES6) and then **Async/Await** (ES8).

### The Promise (The "Future")
A **Promise** is an object representing the eventual completion (or failure) of an asynchronous operation.
A Promise has three states:
1. **Pending:** In progress.
2. **Fulfilled (Resolved):** Success. (Calls `.then()`)
3. **Rejected:** Error. (Calls `.catch()`)

### Async / Await
This is "syntactic sugar" built on top of Promises. It allows you to write asynchronous code that _looks_ and _behaves_ like synchronous blocking code, which makes it incredibly readable for backend engineers.
- **`async`**: Put this in front of a function declaration. It ensures the function returns a Promise.
- **`await`**: Put this in front of a Promise. It pauses the execution of _that specific function_ (not the whole browser) until the Promise resolves.
## Comparison
**Using Raw Promises:**
```javascript
function getData() {
    fetch('https://api.com/user')
        .then(response => response.json())
        .then(data => console.log(data))
        .catch(error => console.error(error));
}
```
**Using Async/Await (Modern Standard):**
```javascript
async function getData() {
    try {
        // "await" pauses here until fetch returns
        const response = await fetch('https://api.com/user');
        
        // "await" pauses here until json parsing finishes
        const data = await response.json();
        
        console.log(data);
    } catch (error) {
        // Standard try/catch handles "Rejections"
        console.error(error);
    }
}
```

# The Node.js Ecosystem
We established that Node is **V8 + C++ Bindings**. But for a developer, the biggest difference is the **Module System** and **Dependency Management**.

## The "Pom.xml" of JS: `package.json`
In Spring Boot, you have Maven (`pom.xml`) or Gradle (`build.gradle`). In Node.js, we have **NPM (Node Package Manager)** and the `package.json` file.
- **`package.json`**: Lists your project metadata and dependencies.
- **`node_modules` folder**: This is where the libraries actually live. Unlike Maven (which stores jars in a central `.m2` folder on your hard drive), NPM installs dependencies **locally** into every single project. This leads to the famous "heavy" node_modules folder.

## Import vs. Require (The Schism)
**A. CommonJS ( The "Old" Node Standard)** For years, Node used a system called CommonJS. It is dynamic and synchronous.

```javascript
// Import
const fs = require('fs');
// Export
module.exports = myFunction;
```
**B. ES Modules (The "New" Standard)** The Browser uses `import` and `export`. To align Node with the Browser, Node now supports ES Modules (ESM). This looks like Java imports.
```javascript
// Import
import fs from 'fs';
// Export
export default myFunction;
```
**The Conflict:** By default, Node.js assumes files are **CommonJS** (`require`). If you try to use `import` in a `.js` file, Node might crash unless you specifically configure `package.json` with `"type": "module"`.

The image above highlights the architecture we discussed: Your JS code talks to V8, which uses **Libuv** (the C++ library) to handle the Event Loop and OS tasks (like Files/Network).