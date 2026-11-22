---
tags:
  - FORMAT
  - JavaScript
---
# Writing Test Cases with Assertions

**Test cases** define expected behavior for code, while **assertions** check if the actual output matches expectations. Assertions are essential in unit testing to validate code correctness.

---

 What Are Assertions?

Assertions are statements that check if a given condition is **true**. If the assertion fails, the test fails.

Example of an assertion:

```javascript
expect(2 + 2).toBe(4); // Passes ✅
expect(2 + 2).toBe(5); // Fails ❌
```

Different testing frameworks have built-in assertion libraries:

* **Jest**: Uses `expect()`

* **Mocha**: Uses `Chai` for assertions (`expect()`, `should()`, `assert()`)

* **Jasmine**: Uses `expect()`

---

 Writing Test Cases in Different Frameworks

# Writing Assertions in Jest

Jest uses the `expect()` function for assertions.

## Basic Example

```javascript
test("Addition works correctly", () => {
    expect(2 + 3).toBe(5);
});
```

## Common Jest Assertions

```javascript
expect(10).toBe(10); // Exact match
expect(10).not.toBe(5); // Inverse match
expect({ a: 1 }).toEqual({ a: 1 }); // Object equality
expect(null).toBeNull(); // Checks for null
expect([1, 2, 3]).toContain(2); // Array contains value
expect("hello").toMatch(/ell/); // String matches regex
```

---

# Writing Assertions in Mocha \+ Chai

Mocha itself does not have assertions, so we use **Chai**.

## Basic Example

```javascript
const { expect } = require("chai");

describe("Addition Function", () => {
    it("should return 5 for 2 + 3", () => {
        expect(2 + 3).to.equal(5);
    });
});
```

## Common Chai Assertions

```javascript
expect(10).to.equal(10); // Exact match
expect(10).to.not.equal(5); // Inverse match
expect({ a: 1 }).to.deep.equal({ a: 1 }); // Object equality
expect(null).to.be.null; // Checks for null
expect([1, 2, 3]).to.include(2); // Array contains value
expect("hello").to.match(/ell/); // String matches regex
```

---

# Writing Assertions in Jasmine

Jasmine also uses `expect()`.

## Basic Example

```javascript
describe("Subtraction Function", () => {
    it("should return 3 for 8 - 5", () => {
        expect(8 - 5).toBe(3);
    });
});
```

## Common Jasmine Assertions

```javascript
expect(10).toBe(10); // Exact match
expect(10).not.toBe(5); // Inverse match
expect({ a: 1 }).toEqual({ a: 1 }); // Object equality
expect(null).toBeNull(); // Checks for null
expect([1, 2, 3]).toContain(2); // Array contains value
expect("hello").toMatch(/ell/); // String matches regex
```

---

 Advanced Assertions

# Checking Objects

Jest:

```javascript
expect({ name: "Alice" }).toEqual({ name: "Alice" });
```

Chai:

```javascript
expect({ name: "Alice" }).to.deep.equal({ name: "Alice" });
```

Jasmine:

```javascript
expect({ name: "Alice" }).toEqual({ name: "Alice" });
```

---

# Checking Arrays

Jest:

```javascript
expect([1, 2, 3]).toContain(2);
```

Chai:

```javascript
expect([1, 2, 3]).to.include(2);
```

Jasmine:

```javascript
expect([1, 2, 3]).toContain(2);
```

---

# Checking Exceptions

Jest:

```javascript
function throwError() {
    throw new Error("Something went wrong");
}
expect(() => throwError()).toThrow("Something went wrong");
```

Chai:

```javascript
const { expect } = require("chai");

function throwError() {
    throw new Error("Something went wrong");
}
expect(() => throwError()).to.throw("Something went wrong");
```

Jasmine:

```javascript
function throwError() {
    throw new Error("Something went wrong");
}
expect(() => throwError()).toThrowError("Something went wrong");
```

---

 Key Takeaways

* Assertions validate that expected and actual values match.

* **Jest, Mocha (Chai), and Jasmine** provide built-in assertion functions.

* Use **`toBe()` for primitive values** and **`toEqual()` for objects**.

* Assertions can check **numbers, strings, arrays, objects, and exceptions**.

Would you like to see how to test **asynchronous functions** with assertions?

