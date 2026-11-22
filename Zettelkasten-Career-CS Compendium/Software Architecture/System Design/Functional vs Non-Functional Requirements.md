---
tags:
  - SystemDesign
---
The difference is straightforward:

- **Functional requirements**
	- Describe **what a system should do**.
	- Specify behavior, features, or interactions.
	- Usually expressed as actions, tasks, or use cases.
	- **Examples:**
		- A user can log in with a username and password.
		- The system sends a confirmation email after registration.#
		- A shopping cart calculates totals including tax and discounts.
- **Non-functional requirements**
	- Describe **how a system should behave** or **constraints on the system**.
	- Focus on quality attributes rather than specific features.
	- **Examples:**
		- The system must respond to any request within 200 ms.
		- The application must handle 10,000 concurrent users.
		- Data must be encrypted in transit and at rest.
		- The system should be available 99.99% of the time.

---

# Key differences

| Aspect            | Functional                          | Non-Functional                                         |
| ----------------- | ----------------------------------- | ------------------------------------------------------ |
| Focus             | Features and behavior               | Performance, security, reliability, usability          |
| Question answered | “What should the system do?”        | “How should the system perform or behave?”             |
| Examples          | Login, search, payment processing   | Response time, scalability, availability, security     |
| Measurement       | Usually binary (implemented or not) | Often measured with metrics (ms, % uptime, throughput) |

---

In short: **functional = what it does, non-functional = how well it does it or under what constraints**.
