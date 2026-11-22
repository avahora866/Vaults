---
tags:
  - Architecture
---

# Monolithic Architecture

**What it is**:  
An application built as one large, interconnected unit where all modules share the same memory and database space.

**Pros**:

- Simple to develop and deploy
- Easier to debug and test
- Well-suited for small teams

**Cons**:

- Harder to scale (you scale the whole app)
- Tight coupling between components
- One failure can crash the whole app
- Long build and deployment cycles

**When to use**:

- MVPs and early-stage startups
- Projects with a small team and limited scope

---

# Microservices Architecture

**What it is**:  
A system built from multiple, independently deployable services, each handling a specific business function.

**Pros**:

- Independent scaling, deployment, and development
- Better fault isolation
- Teams can work in parallel
- Easier to adopt different technologies

**Cons**:

- More complex architecture (networking, orchestration)
- Needs advanced monitoring and DevOps
- Data consistency and cross-service communication is harder

**When to use**:

- Large systems with multiple teams
- When scalability, resilience, and maintainability are crucial

---

# Serverless Architecture

**What it is**:  
An event-driven model where individual functions are executed in response to events, managed by a cloud provider.

**Pros**:

- No server management
- Auto-scaling
- Pay only for execution time
- Fast to deploy and iterate

**Cons**:

- Cold starts may cause latency
- Limited execution time and memory
- Harder to test/debug
- Tight coupling to cloud provider services

**When to use**:

- Lightweight, event-driven tasks
- APIs, background jobs, automation
- Prototyping or low-maintenance backends

---

# Example Analogy

- **Monolithic**: A single restaurant kitchen that handles everything—starters, mains, desserts, drinks—all in one space.
- **Microservices**: A food court with specialized stalls—each one handles only one type of food but works together.
- **Serverless**: A vending machine that prepares and serves food only when someone requests it.