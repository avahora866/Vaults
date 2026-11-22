---
tags:
  - FORMAT
  - SoftwareDesign
---
**Domain-Driven Design (DDD)** is an approach to software development where the primary focus is on the **core domain** and its **business logic**. It aims to align the software design closely with the real-world problem the software is trying to solve. DDD emphasizes collaboration between technical and domain experts to create a model that reflects the domain accurately and drives the software's structure and behavior.

# Core Concepts of DDD

## Domain

The **domain** is the area of knowledge or activity around which the application logic revolves. For example, in a banking system, the domain involves things like accounts, transactions, and balances.

## Model

The **model** is a system of abstractions that describes selected aspects of the domain and can be used to solve problems related to that domain. It is expressed in code and is intended to be a living, evolving representation of domain understanding.

## Ubiquitous Language

The **ubiquitous language** is a common language developed by the team (developers and domain experts) based on the domain model. It should be used in code, discussions, documentation — everywhere. This reduces misunderstandings and ensures the model stays aligned with reality.

## Bounded Context

A **bounded context** defines the boundary within which a particular domain model is defined and applicable. Different parts of a system might have different models even if the same terms are used, but inside a bounded context, terms have specific meanings.

Example: The word "Order" in an e-commerce site could mean a _shopping cart_ in the customer context and a _shipping manifest_ in the logistics context.

## Entities

**Entities** are objects that have a distinct identity that runs through time and different states. Their identity matters more than their attributes.

Example: A `Customer` entity may change addresses or phone numbers, but it's still the same customer.

## Value Objects

**Value Objects** are objects that describe aspects of the domain and do not have an identity. They are defined only by their attributes.

Example: An `Address` could be a value object with fields like street, city, and postcode. Two addresses with the same data are considered the same value object.

## Aggregates and Aggregate Roots

An **Aggregate** is a cluster of domain objects that are treated as a single unit. Each aggregate has a **root** and a boundary.

- The **aggregate root** is the only member of the aggregate that outside objects are allowed to hold references to.
- All access to the aggregate should go through the root.

Example: In an Order Management system, an `Order` might be the aggregate root, and `OrderItems` are internal parts.

## Repositories

**Repositories** are mechanisms for retrieving and storing aggregates. They abstract the persistence layer (e.g., a database) from the domain model.

Example: An `OrderRepository` might have methods like `save(Order order)` and `Order findById(OrderId id)`.

## Services

**Domain Services** are operations or actions that don't naturally fit into an entity or value object but still are part of the domain logic.

Example: A `PaymentService` could handle the logic of transferring funds between accounts.

# Strategic Design

In large systems, DDD suggests using **strategic design** concepts to manage complexity:

- **Context Mapping**: Identifying different bounded contexts and their relationships.
- **Integration Patterns**: Defining how different contexts communicate (e.g., through Anticorruption Layers, Shared Kernels).

# Why Use Domain-Driven Design

- **Better Alignment with Business Needs**: Close collaboration with domain experts ensures the software matches real-world needs.
- **Improved Communication**: Ubiquitous language makes conversations between developers and non-developers clearer.
- **More Maintainable Code**: Codebases modeled around domain logic tend to be more understandable and adaptable to changes.

# When to Use Domain-Driven Design

DDD is not suitable for all projects. It's most beneficial when:

- The domain is **complex** and **changing** frequently.
- There is a **tight collaboration** between developers and domain experts.
- Business logic is **central**, not secondary to technical concerns.

For simple CRUD systems or low-complexity applications, DDD might be overkill.

# Example Walkthrough

Imagine you are building a **Loan Approval System**.

- **Domain**: Loan approvals
- **Entities**: `LoanApplication`, `Customer`
- **Value Objects**: `CreditScore`, `Address`
- **Aggregates**: `LoanApplication` is an aggregate root containing application data and state transitions.
- **Services**: `CreditEvaluationService` to calculate credit scores.
- **Repository**: `LoanApplicationRepository` to save and retrieve applications.

Developers and business analysts would work together to ensure that terms like "Approved", "Rejected", "Pending Review" mean the same thing in code, documentation, and meetings.

# Key DDD Patterns

Some common DDD patterns are:

- **Factory**: Create complex aggregates or entities cleanly.
- **Anti-Corruption Layer**: Protect your model from being polluted by external systems' models.
- **Domain Events**: Raise events when something significant happens in the domain, to enable decoupled reaction.

# Summary

Domain-Driven Design is about **putting the domain at the center** of your software development process. It uses clear modeling, boundary definitions, and deep collaboration with domain experts to produce software that truly solves business problems and is adaptable to change.