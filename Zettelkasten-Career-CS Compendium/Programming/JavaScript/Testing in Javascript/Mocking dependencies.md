---
tags:
  - FORMAT
  - JavaScript
---
# Mocking Dependencies in Unit Testing

**Mocking** is the process of replacing real dependencies in your code with **mock objects** or **stubbed functions** to isolate the unit under test. This allows for more controlled, predictable, and fast tests by removing the need to interact with external resources like databases, APIs, or file systems.

Mocking is essential for unit testing because it ensures that the unit under test is not affected by the behavior of its dependencies.

---

# Why Mock Dependencies?

* **Isolation**: Ensure that the test only checks the unit's behavior, not the behavior of external dependencies.

* **Control**: You can simulate specific behaviors, including edge cases and errors.

* **Speed**: Avoid lengthy network requests, file I/O, or database access, which can slow down tests.

* **Test Unreachable Code**: Some code might be difficult to test in isolation due to complex or non-deterministic behavior. Mocks help simulate various states.

---

# Mocking with Jest

Jest has **built-in support for mocking**. It provides utilities like `jest.fn()`, `jest.mock()`, and `jest.spyOn()` to easily create mocks and spies.

## Mocking Functions with `jest.fn()`

`jest.fn()` allows you to create a mock function. You can specify its behavior using `.mockReturnValue()` or `.mockImplementation()`.

**Example:**

Let's say you have a function that calls a `fetchData()` function, and you want to mock `fetchData()`:

```javascript
function fetchData() {
    return "Real Data";
}

function processData() {
    return fetchData();
}
```

To mock `fetchData()` in a test:

```javascript
test("processData uses mocked fetchData", () => {
    // Mock fetchData
    const mockFetchData = jest.fn().mockReturnValue("Mocked Data");

    // Replace real fetchData with mock
    global.fetchData = mockFetchData;

    const result = processData();

    expect(result).toBe("Mocked Data");
    expect(mockFetchData).toHaveBeenCalled();
});
```

*   
  **`.mockReturnValue()`** specifies what the mock should return.

* **`.toHaveBeenCalled()`** checks if the mock function was called.

---

## Mocking Modules with `jest.mock()`

Jest also allows you to **mock entire modules**. This is useful if you're testing a function that depends on another module or library (e.g., an API call or database).

**Example:**

Suppose you have a module that imports `axios` to fetch data from an API:

```javascript
// api.js
import axios from "axios";

export const fetchUserData = async (userId) => {
    const response = await axios.get(`/user/${userId}`);
    return response.data;
};
```

To mock `axios` in a test:

```javascript
// api.test.js
import axios from "axios";
import { fetchUserData } from "./api";

// Mock the entire axios module
jest.mock("axios");

test("fetchUserData returns mocked data", async () => {
    // Set up the mock to return specific data
    axios.get.mockResolvedValue({ data: { name: "John Doe", age: 30 } });

    const userData = await fetchUserData(1);

    expect(userData.name).toBe("John Doe");
    expect(userData.age).toBe(30);
    expect(axios.get).toHaveBeenCalledWith("/user/1");
});
```

*   
  **`jest.mock("axios")`** mocks the `axios` module, allowing you to specify what `axios.get()` should return.

* **`.mockResolvedValue()`** allows you to specify the resolved value for a promise (useful for asynchronous functions).

---

## Mocking Date/Time with `jest.spyOn()`

Jest allows you to **spy on** and mock methods like `Date.now()` or `setTimeout()` using `jest.spyOn()`.

**Example:**

If you need to mock the current date and time in a test:

```javascript
test("getCurrentTime returns mocked time", () => {
    const mockDate = new Date(2025, 0, 1);
    jest.spyOn(global, "Date").mockImplementation(() => mockDate);

    const currentTime = new Date();

    expect(currentTime.getFullYear()).toBe(2025);
    expect(currentTime.getMonth()).toBe(0); // January
    expect(currentTime.getDate()).toBe(1);

    global.Date.mockRestore(); // Restore the original Date implementation
});
```

*   
  **`jest.spyOn(global, "Date")`** spies on the global `Date` object and allows you to mock it.

* **`.mockImplementation()`** provides the mock behavior.

* **`.mockRestore()`** restores the original behavior after the test.

---

# Mocking with Mocha and Chai (Using Sinon)

Mocha doesn't have built-in mocking, but you can use **Sinon** to create spies, stubs, and mocks.

## Stubbing Functions with Sinon

You can use **Sinon** to replace a function with a mock (stub), which allows you to control its behavior.

**Example:**

Suppose you have the following code:

```javascript
function fetchData() {
    return "Real Data";
}

function processData() {
    return fetchData();
}
```

In a Mocha test, you can use Sinon to mock `fetchData()`:

```javascript
const { expect } = require("chai");
const sinon = require("sinon");

describe("processData", () => {
    it("should return mocked data", () => {
        // Create a stub for fetchData
        const fetchDataStub = sinon.stub().returns("Mocked Data");

        // Replace real fetchData with stub
        const result = processData();

        expect(result).to.equal("Mocked Data");
        expect(fetchDataStub.calledOnce).to.be.true;
    });
});
```

## Mocking HTTP Requests with Sinon

You can use **Sinon** to mock network requests, for example, by intercepting `XMLHttpRequest`.

```javascript
const sinon = require("sinon");
const { expect } = require("chai");

describe("API Call", () => {
    it("should call the API", () => {
        const xhr = sinon.useFakeXMLHttpRequest();
        const requests = [];

        xhr.onCreate = (req) => {
            requests.push(req);
        };

        // Simulate a fake API call
        const fakeRequest = new XMLHttpRequest();
        fakeRequest.open("GET", "/api/data");
        fakeRequest.send();

        // Verify if the request was made
        expect(requests.length).to.equal(1);
        expect(requests[0].url).to.equal("/api/data");

        xhr.restore(); // Restore the original XMLHttpRequest
    });
});
```

*   
  **`sinon.stub()`** is used to replace the real function with a mock that returns specific values.

* **`sinon.useFakeXMLHttpRequest()`** is used to mock HTTP requests.

---

# Key Takeaways

* **Mocking** helps isolate your unit tests by replacing real dependencies with controlled, predictable behavior.

* **Jest** offers built-in mocking capabilities (`jest.fn()`, `jest.mock()`, `jest.spyOn()`).

* **Sinon** is commonly used with **Mocha** for mocking (using **stubs** and **spies**).

* **Mocking HTTP requests, timers, and external services** is essential for testing components that depend on these resources.

Would you like an example for **mocking an external API** or **mocking a database call** in more detail?

