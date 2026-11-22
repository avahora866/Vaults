---
tags:
  - DesignPattern
---
The **Strategy** pattern is a **behavioral** design pattern that enables selecting an algorithm’s behavior at runtime. It defines a family of algorithms, encapsulates each one, and makes them interchangeable within a context object.

# Intent

- Define a set of interchangeable algorithms or behaviors.
- Allow the client to choose the algorithm at runtime without modifying the context class.
- Decouple the behavior from the context.

# Motivation

Instead of hard-coding multiple algorithm variations in a class using conditional logic, the Strategy pattern lets you define each algorithm separately and inject it into the context. This promotes **open/closed principle**—open for extension, closed for modification.

# Structure

1. **Strategy**: Interface common to all supported algorithms.
2. **ConcreteStrategy**: Implements the Strategy interface with specific algorithms.
3. **Context**: Maintains a reference to a Strategy object and delegates it to perform the behavior.

```plaintext
     +--------------+             +----------------+
     |   Context    |-----------> |    Strategy    |
     +--------------+             +----------------+
            |                            /   \
            ↓                           ↓     ↓
    +----------------+        +----------------+ 
    | ConcreteStrategyA|      | ConcreteStrategyB|
    +----------------+        +----------------+
```

# Java Example

Let's implement a payment system where users can choose to pay by credit card or PayPal.

## Step 1: Define the Strategy interface

```java
interface PaymentStrategy {
    void pay(int amount);
}
```

## Step 2: Implement concrete strategies

```java
class CreditCardPayment implements PaymentStrategy {
    private String cardNumber;
    private String name;

    public CreditCardPayment(String cardNumber, String name) {
        this.cardNumber = cardNumber;
        this.name = name;
    }

    public void pay(int amount) {
        System.out.println("Paid " + amount + " using Credit Card: " + cardNumber);
    }
}
```

```java
class PayPalPayment implements PaymentStrategy {
    private String email;

    public PayPalPayment(String email) {
        this.email = email;
    }

    public void pay(int amount) {
        System.out.println("Paid " + amount + " using PayPal account: " + email);
    }
}
```

## Step 3: Create the Context class

```java
class ShoppingCart {
    private PaymentStrategy paymentStrategy;

    public void setPaymentStrategy(PaymentStrategy strategy) {
        this.paymentStrategy = strategy;
    }

    public void checkout(int amount) {
        if (paymentStrategy == null) {
            System.out.println("No payment strategy selected.");
        } else {
            paymentStrategy.pay(amount);
        }
    }
}
```

## Step 4: Client Code

```java
public class StrategyPatternDemo {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();

        cart.setPaymentStrategy(new CreditCardPayment("1234-5678-9012-3456", "Alice"));
        cart.checkout(250);

        cart.setPaymentStrategy(new PayPalPayment("alice@example.com"));
        cart.checkout(100);
    }
}
```

## Output:

```
Paid 250 using Credit Card: 1234-5678-9012-3456
Paid 100 using PayPal account: alice@example.com
```

# Benefits

- **Flexible behavior**: Easily switch algorithms at runtime.
- **Decoupled code**: The context doesn’t need to know implementation details of the strategy.
- **Complies with open/closed principle**: New strategies can be added without modifying existing code.

# Drawbacks

- **Too many classes**: Each strategy requires a separate class, which can lead to class explosion.
- **Client awareness**: The client must be aware of different strategies and when to use each.

# Common Use Cases

- Payment processing systems (as above)
- Sorting strategies (e.g., different sort algorithms)
- Compression formats (zip, rar, tar)
- Pathfinding algorithms (BFS, DFS, A*)
- AI decision-making in games

# Java Built-in Examples

The Strategy pattern is heavily used in:

- **`Comparator` interface** in `Collections.sort()`—you can pass different sorting strategies.
- **Java Cryptography API**—e.g., choosing different encryption algorithms at runtime.
- **Dependency injection** frameworks like Spring, which can inject different strategy beans at runtime.