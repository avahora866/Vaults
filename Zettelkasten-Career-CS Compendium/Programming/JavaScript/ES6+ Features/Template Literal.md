---
tags:
  - FORMAT
  - JavaScript
---
# Template Literals in JavaScript

Template literals, also known as **template strings**, are a feature in JavaScript that provides an easier and more flexible way to work with strings. They allow for string interpolation, multi-line strings, and embedding expressions directly within strings.

---

## 1️⃣ Syntax of Template Literals

Template literals are enclosed by **backticks** (\` \`), as opposed to regular strings which are enclosed by single quotes (`'`) or double quotes (`"`).

### Basic Syntax

```javascript
let string = `This is a template literal`;
```

### Example

```javascript
let message = `Hello, world!`;
console.log(message); // Output: Hello, world!
```

---

## 2️⃣ String Interpolation

One of the most powerful features of template literals is **string interpolation**, which allows you to embed expressions or variables directly into the string.

### Syntax

To insert variables or expressions into a template literal, you use **${}**.

```javascript
let name = "Alice";
let age = 25;
let message = `My name is ${name} and I am ${age} years old.`;

console.log(message); // Output: My name is Alice and I am 25 years old.
```

### Example with Expression

You can even embed expressions like arithmetic operations or function calls within template literals.

```javascript
let num1 = 5;
let num2 = 10;
let result = `The sum of ${num1} and ${num2} is ${num1 + num2}.`;

console.log(result); // Output: The sum of 5 and 10 is 15.
```

---

## 3️⃣ Multi-line Strings

Before template literals, creating multi-line strings in JavaScript was tricky, requiring concatenation or escape characters. Template literals, however, allow for **multi-line strings** by simply using a new line within the backticks.

### Syntax

```javascript
let multiLineString = `This is
a multi-line
string.`;

console.log(multiLineString); 
// Output: 
// This is
// a multi-line
// string.
```

You don't need to use `\n` for line breaks as template literals preserve the line breaks directly.

---

## 4️⃣ Embedding Expressions

You can embed any valid JavaScript expression inside `${}` within a template literal. This includes function calls, mathematical expressions, or even complex logic.

### Example with Function Call

```javascript
function greet(name) {
  return `Hello, ${name}!`;
}

let name = "Bob";
console.log(greet(name)); // Output: Hello, Bob!
```

### Example with Conditional Logic

```javascript
let age = 17;
let message = `You are ${age >= 18 ? "an adult" : "a minor"}.`;

console.log(message); // Output: You are a minor.
```

---

## 5️⃣ Tagged Templates

A more advanced feature of template literals is **tagged templates**, which allow you to parse template literals with a function.

### Syntax

```javascript
function tag(strings, ...expressions) {
  console.log(strings); // Array of string literals
  console.log(expressions); // Array of interpolated expressions
}

let name = "Alice";
let age = 30;

tag`Hello, my name is ${name} and I am ${age} years old.`;
```

### Example with Custom Processing

You can use tagged templates for custom string processing.

```javascript
function uppercase(strings, ...values) {
  let result = "";
  strings.forEach((str, index) => {
    result += str + (values[index] ? values[index].toUpperCase() : "");
  });
  return result;
}

let name = "alice";
let message = uppercase`Hello, my name is ${name}.`;

console.log(message); // Output: Hello, my name is ALICE.
```

---

## 6️⃣ Advantages of Template Literals

1. **Readable and Maintainable Code**: Template literals make the code more readable and easier to maintain, especially when dealing with dynamic strings.

2. **Multi-line Strings**: You can create multi-line strings without using escape characters (`\n`).

3. **Expression Interpolation**: Embed variables, expressions, or function calls directly in strings.

4. **Tagged Templates**: Offers advanced functionality for custom string processing.

---

## 7️⃣ Summary of Template Literals

* **Syntax**: Template literals use backticks (`` ` ``) for string declaration.

* **String Interpolation**: Use `${}` to embed expressions, variables, or function calls within strings.

* **Multi-line Strings**: Template literals preserve line breaks without needing escape sequences like `\n`.

* **Tagged Templates**: Allows processing template literals with a custom function.

Template literals are an excellent tool to make string handling in JavaScript more efficient, cleaner, and more powerful.

