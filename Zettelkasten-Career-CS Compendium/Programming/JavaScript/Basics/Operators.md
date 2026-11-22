---
tags:
  - FORMAT
  - JavaScript
---
# JavaScript Operators

Operators in JavaScript are symbols or keywords that are used to perform operations on values and variables. They allow you to manipulate data and variables to perform various actions like calculations, comparisons, and logical operations.

JavaScript has several types of operators:

## Arithmetic Operators

Arithmetic operators are used to perform mathematical calculations.

| Operator | Description | Example | Result |
| ----- | ----- | ----- | ----- |
| `+` | Addition | `5 + 3` | `8` |
| `-` | Subtraction | `5 - 3` | `2` |
| `*` | Multiplication | `5 * 3` | `15` |
| `/` | Division | `6 / 3` | `2` |
| `%` | Modulus (Remainder) | `5 % 3` | `2` |
| `**` | Exponentiation | `2 ** 3` | `8` |

### Example:

```javascript
let a = 10, b = 5;
console.log(a + b);  // Output: 15
console.log(a - b);  // Output: 5
console.log(a * b);  // Output: 50
console.log(a / b);  // Output: 2
console.log(a % b);  // Output: 0 (Remainder)
console.log(2 ** 3); // Output: 8 (Exponentiation)
```

## Logical Operators

Logical operators are used to perform logical operations, mainly with boolean values (`true` or `false`).

### Common Logical Operators:

| Operator | Description | Example | Result |
| ----- | ----- | ----- | ----- |
| `&&` | Logical AND: returns `true` if both operands are true | `true && false` | `false` |
| \` |  | \` | Logical OR: returns `true` if at least one operand is true |
| `!` | Logical NOT: reverses the boolean value of its operand | `!true` | `false` |

### Example:

```javascript
let a = true, b = false;

console.log(a && b);  // Output: false (AND operation)
console.log(a || b);  // Output: true (OR operation)
console.log(!a);      // Output: false (NOT operation)
```

## Comparison Operators

Comparison operators are used to compare two values and return a boolean result (`true` or `false`).

| Operator | Description | Example | Result |
| ----- | ----- | ----- | ----- |
| `==` | Equal to (value comparison) | `5 == 5` | `true` |
| `===` | Strict equal to (value and type comparison) | `5 === '5'` | `false` |
| `!=` | Not equal to (value comparison) | `5 != 3` | `true` |
| `!==` | Strict not equal to (value and type comparison) | `5 !== '5'` | `true` |
| `>` | Greater than | `5 > 3` | `true` |
| `<` | Less than | `3 < 5` | `true` |
| `>=` | Greater than or equal to | `5 >= 5` | `true` |
| `<=` | Less than or equal to | `3 <= 5` | `true` |

### Example:

```javascript
let x = 10, y = 5, z = '10';

console.log(x == y);    // Output: false (loose equality)
console.log(x === z);   // Output: false (strict equality, different types)
console.log(x != y);    // Output: true
console.log(x > y);     // Output: true
console.log(x <= 10);   // Output: true
```

## Ternary Operator

The **ternary operator** is a shorthand for `if-else` statements. It takes three operands and evaluates to one of two values based on a condition.

### Syntax:

```javascript
condition ? value_if_true : value_if_false;
```

*   
  If the condition is `true`, it returns `value_if_true`.

* If the condition is `false`, it returns `value_if_false`.

### Example:

```javascript
let age = 18;

let canVote = (age >= 18) ? 'Yes' : 'No';
console.log(canVote);  // Output: Yes
```

This is equivalent to:

```javascript
let canVote;
if (age >= 18) {
  canVote = 'Yes';
} else {
  canVote = 'No';
}
```

## Summary of Operators

### Arithmetic Operators:

* `+` (Addition)

* `-` (Subtraction)

* `*` (Multiplication)

* `/` (Division)

* `%` (Modulus)

* `**` (Exponentiation)

### Logical Operators:

* `&&` (AND)

* `||` (OR)

* `!` (NOT)

### Comparison Operators:

* `==` (Equal to)

* `===` (Strict equal to)

* `!=` (Not equal to)

* `!==` (Strict not equal to)

* `>` (Greater than)

* `<` (Less than)

* `>=` (Greater than or equal to)

* `<=` (Less than or equal to)

### Ternary Operator:

* `condition ? value_if_true : value_if_false;`

### Key Notes:

* **Arithmetic operators** are used for mathematical operations.

* **Logical operators** are used to perform logical operations on boolean values.

* **Comparison operators** are used to compare two values and return a boolean (`true` or `false`).

* The **ternary operator** is a concise alternative to `if-else` for conditional assignments.

