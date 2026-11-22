---
tags:
  - FORMAT
  - JavaScript
---
# Unit Testing in JavaScript with Jest, Mocha, and Jasmine

Unit testing is a **software testing technique** that verifies individual units of code (functions, modules, or components) in **isolation**. The goal is to ensure each unit behaves as expected.

---

 Why Unit Testing?

* **Catches bugs early** before deployment.

* **Improves code quality** by enforcing modularity.

* **Ensures maintainability** by preventing regressions.

* **Speeds up development** with confidence in code changes.

---

 Popular JavaScript Testing Frameworks

| Feature | Jest | Mocha | Jasmine |
| ----- | ----- | ----- | ----- |
| Ease of Use | Easy | Moderate | Moderate |
| Built-in Assertions | Yes | No (Uses Chai) | Yes |
| Mocking Support | Yes | No (Uses Sinon) | Yes |
| Snapshot Testing | Yes | No | No |
| Speed | Fast | Slower | Medium |
| Async Testing | Yes | Yes | Yes |

---

 Unit Testing with Jest

Jest is a **fast and popular** testing framework developed by Facebook. It has **built-in assertions, mocking, and snapshot testing**.

# Installing Jest

```
npm install --save-dev jest
```

# Writing a Basic Test

Create a `math.js` module:

```javascript
function add(a, b) {
    return a + b;
}

module.exports = { add };
```

Now, create a `math.test.js` file:

```javascript
const { add } = require("./math");

test("adds 2 + 3 to equal 5", () => {
    expect(add(2, 3)).toBe(5);
});
```

# Running Jest

Add this in `package.json`:

```
"scripts": {
  "test": "jest"
}
```

Run tests:

```
npm test
```

---

 Unit Testing with Mocha \+ Chai

Mocha is a **flexible** test framework, but it needs an assertion library like **Chai**.

# Installing Mocha and Chai

```
npm install --save-dev mocha chai
```

# Writing a Test in Mocha

Create a `math.js` module:

```javascript
function multiply(a, b) {
    return a * b;
}

module.exports = { multiply };
```

Now, create a `math.test.js` file:

```javascript
const { expect } = require("chai");
const { multiply } = require("./math");

describe("Multiply Function", () => {
    it("should return 6 for 2 * 3", () => {
        expect(multiply(2, 3)).to.equal(6);
    });

    it("should return 0 for any number multiplied by 0", () => {
        expect(multiply(10, 0)).to.equal(0);
    });
});
```

# Running Mocha

Add this in `package.json`:

```
"scripts": {
  "test": "mocha"
}
```

Run tests:

```
npm test
```

---

 Unit Testing with Jasmine

Jasmine is a **behavior-driven** testing framework with built-in assertion support.

# Installing Jasmine

```
npm install --save-dev jasmine
```

Initialize Jasmine:

```
npx jasmine init
```

# Writing a Jasmine Test

Create a `math.js` module:

```javascript
function subtract(a, b) {
    return a - b;
}

module.exports = { subtract };
```

Now, create a `math.spec.js` file:

```javascript
const { subtract } = require("./math");

describe("Subtract Function", () => {
    it("should return 5 for 10 - 5", () => {
        expect(subtract(10, 5)).toBe(5);
    });

    it("should return -5 for 5 - 10", () => {
        expect(subtract(5, 10)).toBe(-5);
    });
});
```

# Running Jasmine

Add this in `package.json`:

```
"scripts": {
  "test": "jasmine"
}
```

Run tests:

```
npm test
```

---

 Mocking & Spies in Unit Testing

Mocking allows us to replace **real dependencies** with **fake ones** to isolate the unit under test.

# Mocking with Jest

Jest has built-in mocking support:

```javascript
const fetchData = jest.fn(() => "mocked data");

test("fetchData returns mocked data", () => {
    expect(fetchData()).toBe("mocked data");
});
```

# Mocking with Sinon (For Mocha/Chai)

Install Sinon:

```
npm install --save-dev sinon
```

Mock a function:

```javascript
const sinon = require("sinon");

const myFunction = sinon.stub().returns(42);

console.log(myFunction()); // 42
```

---

 Handling Asynchronous Tests

 Jest (Async/Await)

```javascript
async function fetchData() {
    return "data received";
}

test("fetchData returns data", async () => {
    const data = await fetchData();
    expect(data).toBe("data received");
});
```

 Mocha (Async/Await)

```javascript
it("should return data", async () => {
    const data = await fetchData();
    expect(data).to.equal("data received");
});
```

---

 Snapshot Testing with Jest

Jest can **store and compare** UI component snapshots.

```javascript
const renderer = require("react-test-renderer");
const MyComponent = require("./MyComponent");

test("renders correctly", () => {
    const tree = renderer.create(<MyComponent />).toJSON();
    expect(tree).toMatchSnapshot();
});
```

---

 Key Takeaways

* **Jest is best for simplicity and speed** (great for React apps).

* **Mocha \+ Chai gives flexibility** (better for complex test setups).

* **Jasmine is easy to set up with built-in assertions.**

* **Mocking and spies help isolate tests from external dependencies.**

* **Snapshot testing is useful for UI components.**

Would you like an example for **testing a real API** or a **React component**?

