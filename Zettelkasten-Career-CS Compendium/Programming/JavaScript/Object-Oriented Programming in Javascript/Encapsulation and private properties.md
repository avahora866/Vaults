---
tags:
  - FORMAT
  - JavaScript
---
# Encapsulation in JavaScript

Encapsulation is an object-oriented programming (OOP) principle that restricts direct access to some of an object's components, helping to protect the integrity of data and improve modularity. In JavaScript, encapsulation is implemented using objects and closures, ensuring that certain properties and methods remain private and cannot be modified directly from outside the object.

Encapsulation allows you to:

* Hide internal implementation details.

* Control access to data by exposing only necessary methods.

* Improve maintainability and security of the code.

---

# Private Properties in JavaScript

JavaScript provides several ways to implement private properties:

## Using Closures (Pre-ES6)

Before ES6, private properties were typically implemented using closures inside a function. Here's an example:

```javascript
function Person(name) {
    let _name = name; // Private variable

    this.getName = function() {
        return _name;
    };

    this.setName = function(newName) {
        _name = newName;
    };
}

const person = new Person("John");
console.log(person.getName()); // John
console.log(person._name); // Undefined (private)
person.setName("Doe");
console.log(person.getName()); // Doe
```

In this approach, `_name` is not accessible directly from outside the function.

---

## Using ES6 Symbols

Symbols provide a unique key that prevents external access to object properties.

```javascript
const _name = Symbol("name");

class Person {
    constructor(name) {
        this[_name] = name; // Private property
    }

    getName() {
        return this[_name];
    }
}

const person = new Person("Alice");
console.log(person.getName()); // Alice
console.log(person._name); // Undefined
console.log(person[Symbol("name")]); // Undefined (different Symbol reference)
```

Symbols prevent accidental access but do not completely enforce privacy.

---

## Using ES2020 Private Fields (`#`)

th ES2020, JavaScript introduced `#` as a syntax for truly private fields.

```javascript
class Person {
  #name; // Private field

    constructor(name) {
      this.#name = name;
    }

    getName() {
      return this.#name;
    }
}

const person = new Person("Eve");
console.log(person.getName()); // Eve
nsole.log(person.#name); // SyntaxError: Private field '#name' must be declared in an enclosing class
```

re, `#name` is completely inaccessible from outside the class.

---

# Encapsulation in Action

By combining private properties and methods, encapsulation can enforce data integrity and restrict unintended modifications.

```javascript
class BankAccount {
  #balance;

    constructor(initialBalance) {
      this.#balance = initialBalance;
    }

    deposit(amount) {
        if (amount > 0) {
          this.#balance += amount;
        }
    }

    withdraw(amount) {
      if (amount > 0 && amount <= this.#balance) {
          this.#balance -= amount;
        }
    }

    getBalance() {
      return this.#balance;
    }
}

const account = new BankAccount(1000);
console.log(account.getBalance()); // 1000
account.deposit(500);
console.log(account.getBalance()); // 1500
nsole.log(account.#balance); // SyntaxError: Private field '#balance' must be declared in an enclosing class
```

---

# Conclusion

* **Closures** (Pre-ES6) → Used to simulate private properties.

* **Symbols** (ES6) → Provide semi-private properties.

rivate Fields (`#`) (ES2020) → Enforce true encapsulation.

ing `#` private fields is the best modern approach, as it offers strong privacy and is supported in modern JavaScript engines.

