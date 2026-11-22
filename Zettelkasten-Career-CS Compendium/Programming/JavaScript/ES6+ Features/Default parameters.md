---
tags:
  - FORMAT
  - JavaScript
---
# Default Parameters in JavaScript

In JavaScript, **default parameters** allow you to set default values for function parameters when no value or `undefined` is passed for that parameter. This feature makes functions more flexible and helps avoid potential errors when required arguments are missing.

---

## 1️⃣ Syntax of Default Parameters

You can assign default values to function parameters by using the **assignment operator (`=`)** in the function definition.

### Syntax

```javascript
function functionName(parameter1 = defaultValue, parameter2 = defaultValue2) {
  // function body
}
```

---

## 2️⃣ Example of Default Parameters

Here’s a basic example where we define default values for function parameters:

```javascript
function greet(name = "Guest", age = 18) {
  console.log(`Hello, ${name}! You are ${age} years old.`);
}

greet("Alice", 25);  // Output: Hello, Alice! You are 25 years old.
greet("Bob");        // Output: Hello, Bob! You are 18 years old.
greet();             // Output: Hello, Guest! You are 18 years old.
```

### Explanation

* When calling `greet("Alice", 25)`, both `name` and `age` are passed, so the function uses the provided values.

* When calling `greet("Bob")`, `name` is provided, but `age` is omitted. Since `age` has a default value of `18`, it uses the default.

* When calling `greet()`, both parameters are omitted, so the default values `"Guest"` and `18` are used.

---

## 3️⃣ Default Parameters with Expressions

The default value doesn’t have to be a simple constant. It can also be an expression, function call, or even another variable.

### Example with Expression

```javascript
function multiply(a, b = a * 2) {
  return a * b;
}

console.log(multiply(3));   // Output: 9 (b defaults to a*2, i.e., 6)
console.log(multiply(3, 4)); // Output: 12 (b is 4, so it uses that value)
```

### Explanation

In the function `multiply()`, if the second parameter `b` is not provided, it defaults to the value `a * 2`. This allows dynamic behavior based on the value of `a`.

---

## 4️⃣ Default Parameters and `undefined`

The default parameter value is only assigned if the argument passed is **`undefined`**. If the argument is explicitly set to `null` or any other falsy value (e.g., `0`, `false`, `""`), the default value will **not** be used.

### Example with `undefined`

```javascript
function greet(name = "Guest") {
  console.log(`Hello, ${name}!`);
}

greet("Alice");     // Output: Hello, Alice!
greet(undefined);   // Output: Hello, Guest!
greet(null);        // Output: Hello, null!
greet("");          // Output: Hello, ! (Empty string is treated as a value, not undefined)
```

### Explanation

* When calling `greet(undefined)`, the default value `"Guest"` is used, because `undefined` explicitly triggers the default value.

* When calling `greet(null)` or `greet("")`, the default is not applied, as `null` and empty strings are considered valid values in JavaScript.

---

## 5️⃣ Default Parameters with Rest Parameters

Default parameters can also be used in functions that accept **rest parameters** (`...args`). The rest parameter collects all remaining arguments into an array, and you can provide a default value for it.

### Example with Rest Parameters

```javascript
function logMessage(message, ...others = []) {
  console.log(message);
  console.log(others);
}

logMessage("Hello");               // Output: Hello, []
logMessage("Hello", "World");      // Output: Hello, ["World"]
logMessage("Hello", "World", "!");
```

### Explanation

* The rest parameter `...others` will collect all arguments after the first one into an array. If no additional arguments are passed, it defaults to an empty array.

---

## 6️⃣ Best Practices for Default Parameters

* **Clarity and readability**: Default parameters improve function readability by specifying what values will be used if no argument is provided.

* **Avoiding `undefined` checks**: Default parameters can help avoid explicit checks for `undefined` or other falsy values inside the function.

* **Use expressions for dynamic defaults**: You can use expressions or even function calls as default values, making your code more flexible.

---

## 7️⃣ Summary of Default Parameters

* **Syntax**: Use the assignment operator (`=`) to set default values in function parameters.

* **Behavior**: Default values are used when an argument is either **not passed** or explicitly passed as **`undefined`**.

* **Dynamic Defaults**: You can use expressions, variables, or even function calls as default parameter values.

* **Falsy Values**: Default parameters are not used if the argument is provided and has a falsy value like `null`, `0`, or `false`.

Default parameters in JavaScript make function definitions cleaner and reduce the need for additional checks inside the function body. This helps in writing more concise and flexible code.

