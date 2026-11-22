---
tags:
  - FORMAT
  - JavaScript
---
# Type Coercion and Type Conversion in JavaScript

In JavaScript, **type coercion** and **type conversion** refer to the process of converting values between different data types. These processes are essential to understand for avoiding unexpected behavior when working with JavaScript’s dynamic type system.

## Type Coercion

Type coercion is the automatic or implicit conversion of a value from one data type to another by JavaScript. This often happens when values of different types are used together in expressions. JavaScript tries to convert the operands to a common type before performing the operation.

### Implicit Type Coercion

JavaScript automatically converts values to the appropriate type when necessary. Here are some common cases where implicit coercion occurs:

### Coercion with `+` Operator

The `+` operator is overloaded in JavaScript. It can either add numbers or concatenate strings, depending on the data types of the operands.

* **When one or both operands are strings**, JavaScript coerces the other operand to a string and concatenates the values.

* **When both operands are numbers**, it performs addition.

#### Example:

```javascript
let num = 5;
let str = "10";

console.log(num + str);  // Output: "510" (number 5 is coerced to string and concatenated)
console.log(num + Number(str)); // Output: 15 (string "10" is coerced to a number)
```

### Coercion with Comparison Operators

When using comparison operators (such as `==`), JavaScript automatically coerces the operands to the same type before performing the comparison.

#### Example:

```javascript
console.log(5 == '5');  // Output: true (string '5' is coerced to number)
console.log(true == 1); // Output: true (boolean true is coerced to number 1)
console.log(false == 0); // Output: true (boolean false is coerced to number 0)
```

### Coercion in Logical Operations

In logical operations, JavaScript coerces values to either `true` or `false` based on their "truthy" or "falsy" nature.

* **Falsy values**: `0`, `""`, `null`, `undefined`, `NaN`, and `false`.

* **Truthy values**: Everything else.

#### Example:

```javascript
console.log(0 || 'Hello'); // Output: "Hello" (0 is falsy, so the second operand is returned)
console.log('' && 'World'); // Output: "" (empty string is falsy, so it is returned)
```

### Coercion with Arithmetic Operators

For arithmetic operations, JavaScript converts operands to numbers (if possible) to perform the operation.

#### Example:

```javascript
console.log('5' - 3);   // Output: 2 (string '5' is coerced to number)
console.log('5' * 3);   // Output: 15 (string '5' is coerced to number)
console.log('5' / 2);   // Output: 2.5 (string '5' is coerced to number)
```

However, with the `+` operator, JavaScript performs **string concatenation** if one of the operands is a string.

```javascript
console.log('5' + 3);   // Output: "53" (number 3 is coerced to string and concatenated)
```

---

## Type Conversion

Type conversion is the explicit conversion of a value from one type to another, which is done intentionally by the programmer using built-in JavaScript functions or methods.

### Converting to a Number

You can explicitly convert values to numbers using the `Number()` function, the unary plus (`+`), or `parseInt()` and `parseFloat()` for specific conversions.

#### Examples:

```javascript
console.log(Number('10'));       // Output: 10 (string '10' to number)
console.log(+ '10');             // Output: 10 (unary plus operator)
console.log(parseInt('10.5'));   // Output: 10 (converts string to integer)
console.log(parseFloat('10.5')); // Output: 10.5 (converts string to float)
```

### Converting to a String

You can explicitly convert values to strings using the `String()` function or by concatenating with an empty string.

#### Examples:

```javascript
console.log(String(10));        // Output: "10" (number 10 to string)
console.log(10 + '');           // Output: "10" (number 10 converted to string)
console.log(String(true));      // Output: "true" (boolean true to string)
```

### Converting to a Boolean

You can convert a value to a boolean using the `Boolean()` function, or by using coercion (like in logical operations).

* **Falsy values**: `0`, `""`, `null`, `undefined`, `NaN`, `false`

* **Truthy values**: Everything else

#### Examples:

```javascript
console.log(Boolean(0));        // Output: false (0 is falsy)
console.log(Boolean('hello'));  // Output: true (non-empty string is truthy)
console.log(Boolean(''));       // Output: false (empty string is falsy)
console.log(Boolean(null));     // Output: false (null is falsy)
```

---

## Key Differences Between Type Coercion and Type Conversion

| Feature | Type Coercion | Type Conversion |
| ----- | ----- | ----- |
| **Definition** | Automatic or implicit conversion of a value by JavaScript. | Explicit conversion done by the programmer. |
| **When It Happens** | Happens automatically when values are used in an operation or expression. | Happens when you use built-in functions like `Number()`, `String()`, etc. |
| **Examples** | `5 + '10'` (Coercion to string) | `Number('10')` (Explicit conversion to number) |
| **Control** | Happens without the programmer's control, can lead to unexpected results. | Programmer-controlled and intentional. |

---

## Conclusion

* **Type Coercion** occurs automatically and can sometimes result in unexpected behavior when JavaScript tries to convert different types to be compatible in an operation. It can be both implicit and automatic.

* **Type Conversion** is the process of explicitly converting a value from one type to another using JavaScript’s built-in methods like `String()`, `Number()`, or `Boolean()`.

Both processes are essential for understanding how JavaScript handles different data types, and knowing when and how to use them can help you avoid bugs and write cleaner, more predictable code.

