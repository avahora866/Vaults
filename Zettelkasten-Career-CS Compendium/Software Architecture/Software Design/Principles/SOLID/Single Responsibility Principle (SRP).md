---
tags:
  - FORMAT
  - SoftwareDesign
  - "#SECONDARY_BRANCH"
---
**Definition**: A class should have only one reason to change.

This means that a class should only have **one responsibility** or **one job**. In other words, it should only do one thing—and all of its behaviour should be narrowly aligned with that one thing.

# What is a "Responsibility"?

A **responsibility** is essentially a **reason for change**. If you can identify more than one reason a class might need to change, then it likely has more than one responsibility.

For example, let’s say you have a `ReportManager` class that:
- Calculates financial data
- Formats the data into a report
- Sends that report to a printer

That class has **three responsibilities**, meaning it has **three reasons to change**:
1. If the way you calculate data changes.
2. If the formatting requirements change.
3. If the printing process changes.

Each of those concerns might evolve independently. If they are all handled in one class, changes to one concern might accidentally break another. This tightly couples unrelated functionality and makes the system harder to maintain, test, and extend.

# Why SRP Matters

1. **Improved Maintainability**: Changes become easier and safer when each class has a focused purpose.
2. **Easier Testing**: Small, single-purpose classes are easier to unit test.
3. **Better Reusability**: Code focused on one thing is more likely to be reused elsewhere.
4. **Low Coupling / High Cohesion**: SRP encourages high cohesion (everything in the class is related) and low coupling (less dependence on unrelated concerns).

# How to Apply It

Let’s take the previous `ReportManager` example and refactor it using SRP:

```java
public class FinancialCalculator {
    public ReportData calculate() {
        // Perform calculations
    }
}

public class ReportFormatter {
    public String format(ReportData data) {
        // Convert data to a formatted report
    }
}

public class ReportPrinter {
    public void print(String report) {
        // Send report to the printer
    }
}
```

Each class now has **a single, clear responsibility**:
- `FinancialCalculator` knows how to **calculate** data.
- `ReportFormatter` knows how to **format** that data.
- `ReportPrinter` knows how to **print** the report.

If printing logic changes, only `ReportPrinter` is affected. The rest of the system remains untouched.

# Common Signs of SRP Violation

- A class changes for many different reasons.
- You feel the need to "comment off" multiple unrelated sections during maintenance.
- The class name contains "and" or is vague (e.g., `UserManagerAndNotifier` or `Utils`).
- The class grows large and hard to test in isolation.

# SRP and Real-World Analogy

Think of SRP like job roles:
- A chef cooks.
- A waiter serves.
- A cashier handles payment.

If one person tries to do all three at once, they’re stretched thin, and service suffers. Same with code.