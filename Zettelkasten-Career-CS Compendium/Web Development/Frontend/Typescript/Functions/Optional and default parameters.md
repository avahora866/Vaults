---
tags:
  - FORMAT
  - Typescript
---
# Optional and Default Parameters in TypeScript

In TypeScript, **optional parameters** and **default parameters** are ways to make function arguments more flexible. These features help you handle scenarios where certain arguments may not always be provided.

---

 Optional Parameters

Optional parameters allow a function to accept **undefined** as a valid argument for specific parameters.  
 This means that the parameter **may or may not be passed** when calling the function.

# Syntax for Optional Parameters

To mark a parameter as optional, append a `?` after the parameter name in the function signature.

# Example: Optional Parameters

```
function greet(name: string, greeting?: string): void {
    console.log(greeting ? `${greeting}, ${name}!` : `Hello, ${name}!`);
}

greet("Alice");           // Output: "Hello, Alice!"
greet("Bob", "Goodbye");  // Output: "Goodbye, Bob!"
```

*   
  In this example, the parameter `greeting` is optional.

* If `greeting` is not provided, TypeScript allows the function to still run with the default behavior.

# Key Points About Optional Parameters:

* Optional parameters must come **after** required parameters in the function signature.

* TypeScript infers that the parameter type can be `undefined` (i.e., `string | undefined`).

# Example with Type Inference:

```
function sum(a: number, b?: number): number {
    return a + (b || 0);  // If b is undefined, it defaults to 0
}

console.log(sum(5));         // Output: 5
console.log(sum(5, 10));     // Output: 15
```

*   
  Here, `b` is optional, and if it's not provided, `b` defaults to `0` inside the function.

---

 Default Parameters

Default parameters allow a function parameter to have a **default value** if no argument is provided when calling the function. This eliminates the need for manually checking if a value is passed.

# Syntax for Default Parameters

To create a default parameter, assign a default value in the function signature.

# Example: Default Parameters

```
function greet(name: string, greeting: string = "Hello"): void {
    console.log(`${greeting}, ${name}!`);
}

greet("Alice");           // Output: "Hello, Alice!"
greet("Bob", "Goodbye");  // Output: "Goodbye, Bob!"
```

*   
  In this example, `greeting` has a default value of `"Hello"`. If not provided, it will use this default value.

# Key Points About Default Parameters:

* Default parameters can be used in any position in the parameter list.

* Default parameters do not require the `?` syntax since they already have a **fallback value**.

---

 Combining Optional and Default Parameters

You can **combine** both optional and default parameters, but the default parameters must still come after the optional ones.

# Example: Combining Optional and Default Parameters

```
function displayInfo(name: string, age?: number, city: string = "Unknown"): void {
    console.log(`${name} is ${age ?? "of unknown age"} and lives in ${city}.`);
}

displayInfo("Alice");                // Output: "Alice is of unknown age and lives in Unknown."
displayInfo("Bob", 25);              // Output: "Bob is 25 and lives in Unknown."
displayInfo("Charlie", 30, "London"); // Output: "Charlie is 30 and lives in London."
```

*   
  Here, `age` is optional, and `city` has a default value of `"Unknown"`.

* If `city` is not provided, it defaults to `"Unknown"`.

# Order of Parameters

* **Optional parameters must come after required parameters** and **default parameters** must be placed after optional parameters.

---

 Optional and Default Parameters with Destructuring

If you use destructuring in function parameters, you can also make specific properties optional or give them default values.

# Example: Destructuring with Optional and Default Parameters

```
function greet({ name, greeting = "Hello" }: { name: string, greeting?: string }): void {
    console.log(`${greeting}, ${name}!`);
}

greet({ name: "Alice" });                // Output: "Hello, Alice!"
greet({ name: "Bob", greeting: "Hi" });  // Output: "Hi, Bob!"
```

*   
  Here, `greeting` is optional, and it defaults to `"Hello"` if not provided.

* Destructuring allows you to set default values for specific properties inside an object.

---

 Practical Use Cases

# Optional Parameters in Functions with Variable Length Arguments

Optional parameters are useful when you want to allow a function to accept a variable number of arguments.

# Example: Summing Numbers with Optional Parameters

```
function sumNumbers(a: number, b?: number, c?: number): number {
    return a + (b ?? 0) + (c ?? 0);
}

console.log(sumNumbers(5));         // Output: 5
console.log(sumNumbers(5, 10));     // Output: 15
console.log(sumNumbers(5, 10, 15)); // Output: 30
```

*   
  `b` and `c` are optional parameters, making the function flexible for different numbers of arguments.

# Default Parameters for Configuration Settings

Default parameters are ideal for functions that have configuration options, where you want to use default values unless specified otherwise.

# Example: Configuring a User Profile

```
function createProfile(name: string, age: number = 18, country: string = "USA"): void {
    console.log(`Name: ${name}, Age: ${age}, Country: ${country}`);
}

createProfile("Alice");                 // Output: "Name: Alice, Age: 18, Country: USA"
createProfile("Bob", 30, "Canada");     // Output: "Name: Bob, Age: 30, Country: Canada"
createProfile("Charlie", 25);           // Output: "Name: Charlie, Age: 25, Country: USA"
```

*   
  Here, `age` and `country` have default values but can be overridden when provided.

---

 Key Differences Between Optional and Default Parameters

| Feature | Optional Parameter | Default Parameter |
| ----- | ----- | ----- |
| **Syntax** | `parameter?` | `parameter = defaultValue` |
| **Behavior** | May be omitted, treated as `undefined` | Must be provided with a default value if omitted |
| **Type** | Type includes `undefined` | Type is the specified type, with default fallback |
| **Usage** | Useful for optional arguments in a function | Useful for parameters that have a reasonable default value |

---

# Final Thoughts

* **Optional parameters** are used when a parameter is **not always required**.

* **Default parameters** are used when you want to provide a **fallback value** for a parameter if it's not provided.

* **Combining both** allows flexibility in your function signatures and helps handle cases where some arguments might be missing or have default behavior.

Would you like to see more examples or have questions about other function features in TypeScript? 😊

