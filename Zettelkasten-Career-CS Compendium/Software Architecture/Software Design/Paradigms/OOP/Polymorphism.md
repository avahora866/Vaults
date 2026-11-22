---
tags:
  - OOP
  - "#SECONDARY_BRANCH"
---
Polymorphism is about “taking on many shapes”. It's about designing your use cases and algorithms in such a way that we always get the same high-level behaviour, but we allow for dynamic low-level behaviour at runtime.

Let's demonstrate with an example that will let me introduce you to the idea of roles from Responsibility-Driven Design (the actual way to do OO correctly as far as I know). 
Think about the role of a clerk at a grocery store. What’s a clerk supposed to do? 
- Answer questions
- Scan products
- Bag your items
- Prompt you for payment
So you could say those are the responsibilities of a clerk.

![A clerk CRC card that defines the role of a clerk, the related responsibilities and the collaborations it has.](https://d33wubrfki0l68.cloudfront.net/f75bb535112bbdbc2b9e03db6fbbba8c6da9fed8/60591/img/blog/object-oriented/programming/4-principles/clerk-role-responsibility-collaborations-crc-card.png)

A clerk CRC card that defines the role of a clerk, the related responsibilities and the collaborations it has.

Now let’s say that there are 4 different employees that work at this grocery store. Each one can take on the role of a clerk. There’s James who is generally pretty snappy, Mariah who can be somewhat brooding, Max who is always on time (and the friendliest person you’ve ever met), and Mark who is actually the manager but will sometimes fill in when they’re short staffed.

It’s clear that each of the employees does their job slightly differently, but they still perform the responsibilities that a clerk should perform. That is, they are concrete classes which implement the contract of a `Clerk`.

Let's represent the contract using an interface.

```java
// Interface
public interface Clerk {
    Answer answerQuestion(String question);
    void scanProduct(Product product);
    void bagProduct(Product product);
    CompletableFuture<PaymentResult> promptForPayment(Customer customer);
}
```

Then, we could realise the concretions by implementing the `Clerk` interface, handling things with slightly different behaviour if we wish.

```java
// ClerkJames implementation
public class ClerkJames implements Clerk {

    @Override
    public Answer answerQuestion(String question) {
        // Implement uniquely to James
        return new Answer();
    }

    @Override
    public void scanProduct(Product product) {
        // Implement uniquely to James
    }

    @Override
    public void bagProduct(Product product) {
        // Implement uniquely to James
    }

    @Override
    public CompletableFuture<PaymentResult> promptForPayment(Customer customer) {
        // Implement uniquely to James
        return CompletableFuture.completedFuture(new PaymentResult());
    }
}

// ClerkMariah implementation
public class ClerkMariah implements Clerk {

    @Override
    public Answer answerQuestion(String question) {
        // Implement uniquely to Mariah
        return new Answer();
    }

    @Override
    public void scanProduct(Product product) {
        // Implement uniquely to Mariah
    }

    @Override
    public void bagProduct(Product product) {
        // Implement uniquely to Mariah
    }

    @Override
    public CompletableFuture<PaymentResult> promptForPayment(Customer customer) {
        // Implement uniquely to Mariah
        return CompletableFuture.completedFuture(new PaymentResult());
    }
}


```

And then finally, in the code that relies on a `Clerk`, we'd provide a `Clerk` and a `Customer` to check out.

```java
public class Checkout {

    public static CompletableFuture<PaymentResult> checkoutCustomer(Clerk clerk, Customer customer) {
        // Answer all questions
        for (String question : customer.getQuestions()) {
            clerk.answerQuestion(question);
        }

        // Scan and bag all products
        for (Product product : customer.getProductsInCart()) {
            clerk.scanProduct(product);
            clerk.bagProduct(product);
        }

        // Prompt for payment asynchronously
        return clerk.promptForPayment(customer);
    }
}
```

Note that this function relies on a `Clerk`, not a `ClerkJames` or a `ClerkMariah` (or so on). Therefore, it doesn’t matter who shows up to work to fill the role. It could be James, Mariah, Max, or Mark — doesn’t matter. All that matters is that some object with the role of clerk is supplied and that fully implements the requirements of the contract for a `Clerk`:

```java
public class Main {
    public static void main(String[] args) throws Exception {
        // Create a customer
        Customer customer = new Customer();
        // You would initialize customer questions, products, etc.

        // Create clerks
        Clerk clerkJames = new ClerkJames();
        Clerk clerkMariah = new ClerkMariah();

        // Checkout with ClerkJames
        CompletableFuture<PaymentResult> resultJames = Checkout.checkoutCustomer(clerkJames, customer);
        PaymentResult paymentJames = resultJames.get(); // Wait for completion
        System.out.println("Payment processed by James: " + paymentJames);

        // Checkout with ClerkMariah
        CompletableFuture<PaymentResult> resultMariah = Checkout.checkoutCustomer(clerkMariah, customer);
        PaymentResult paymentMariah = resultMariah.get(); // Wait for completion
        System.out.println("Payment processed by Mariah: " + paymentMariah);
    }
}

```

 By relying on a contract instead of a concretion, we gain the ability to sub in for different possible implementations.

This is what polymorphism is about. Dynamic runtime behaviour. Substitutability.

![https://d33wubrfki0l68.cloudfront.net/ca8700e255bc0277325babe712f7a1a34cad5f7f/7f5bb/img/blog/object-oriented/programming/4-principles/clerk.svg](https://d33wubrfki0l68.cloudfront.net/ca8700e255bc0277325babe712f7a1a34cad5f7f/7f5bb/img/blog/object-oriented/programming/4-principles/clerk.svg)

The idea of relying on roles/contracts can be very effective. Within the context of a clean/hexagonal/ports and adapters architecture, the most common reasons you'll do this is to:

- Keep core code separate from infrastructure code using [[Dependency Inversion Principle (DIP)]]
- Keep application layer use cases pure so that they can be unit tested. 
- Swap out an infrastructure dependency (`StripeAPI`, `PaypalAPI`) with a test double (`MockPaymentAPI`) so that we don't make real calls to external services or infrastructure during testing.