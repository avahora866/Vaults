---
tags:
  - Spring
---
This is a programming techniques baased on a concept of an Aspect: 
**Ascpect:** encapsualtes cross-cutting logic
**Cross-cutting logic:** Logic that cuts across the whole applications (Logging, Secuirty)
Aspects can be reused at multiples locations and the same aspect/class can be applied to diffrent classes based on configuration
# Addvantages
- Code for aspect is defined in a single class
- Business code in your applciation is cleaner
- It is configurable (apply diffrent aspects to diffrent classes)
# Disadvanatages
- Too manat aspects makes the app flow hard to follow
- minor perfromance cost for aspect execution (run-time weaving)
# AOP Use Cases
- Most common: logging, security, transactions
- Audit logging: who, what, when, where
- Exception handeling: log exceptions and notify DevOps team via SMS/email
- API Management: how many times a method been called, analytics (peak times, average load, top user)