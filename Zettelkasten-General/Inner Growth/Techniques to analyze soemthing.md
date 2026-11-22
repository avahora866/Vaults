---
tags:
  - InnerGrowth
  - TRUNK
---
# 1. Structural and Quantitative Analysis (The "What" and "How Much")
Use these for rigorous work in software engineering, physics, and finance.

* **Decomposition (First Principles):** Break a complex problem down to its fundamental truths (axioms) and rebuild from there.
    * *Use case:* Optimizing a slow Java application or deriving a physics formula. Do not reason by analogy ("we do it this way because others do").
* **Functional Decomposition:** Analyze a system based on what it *does*, not what it is. Break large processes into sub-processes.
    * *Use case:* Breaking a monolith into microservices.
* **Quantitative Analysis (Regression & Time Series):** Use statistics to define relsationships between variables and predict future trends based on history.
    * *Use case:* Forecasting stock prices or analyzing sensor data in robotics.
* **Morphological Analysis:** List all parameters of a problem and every possible state for those parameters to find new combinations.
    * *Use case:* Generating new game mechanics or engineering solutions.

# 2. Causal and Diagnostic Analysis (The "Why")
Use these when a system fails or behaves unexpectedly.

* **Root Cause Analysis (The 5 Whys):** Drill down by asking "Why?" repeatedly until you find the fundamental flaw, rather than treating the symptom.
    * *Use case:* Debugging a runtime error.
    * *Example:* Server crashed -> Out of memory -> Memory leak -> Unclosed stream in code.
* **Fishbone (Ishikawa) Diagram:** Categorize potential causes (Man, Machine, Material, Method) to see the full picture of a failure.
    * *Use case:* Investigating why a specific build failed in a CI/CD pipeline.

# 3. Systems and Strategic Analysis (The "Big Picture")
Use these to understand how parts interact or to make high-level comparisons.

* **Systems Analysis (Feedback Loops):** Identify how outputs circle back to become inputs (positive or negative feedback).
    * *Use case:* Designing control systems for robotics or understanding market bubbles.
* **Gap Analysis:** Compare current performance with desired performance to identify the steps needed to close the gap.
    * *Use case:* Career planning (Current skills vs. Skills needed for a Quantitative Developer role).
* **Black Box Analysis:** Assess inputs and outputs without regarding the internal workings.
    * *Use case:* Unit testing or reverse engineering.

# 4. Daily Mental Models (The "Quick Check")
Use these for personal decisions, time management, and habit formation.

* **Inversion:** Instead of trying to achieve a positive outcome, analyze how to guarantee a negative one, and avoid those steps.
    * *Use case:* "How do I ruin my health?" -> Stop sleeping, eat sugar, stop moving. (Then, avoid these).
* **Second-Order Thinking:** Analyze the consequence of the consequence. Ask, "And then what?"
    * *Use case:* Buying a high-maintenance car.
        * *1st Order:* It looks cool.
        * *2nd Order:* Mechanics are expensive and parts are rare.
        * *3rd Order:* You lose money and time, preventing you from investing in your hobbies.
* **The Eisenhower Matrix:** Filter tasks by Urgency vs. Importance.
    * *Use case:* Deciding whether to answer an email (Urgent/Not Important) or study math (Not Urgent/Important).
* **Opportunity Cost:** Analyze the value of what you *don't* choose.
    * *Use case:* The cost of 2 hours of drawing is not just time; it is 2 hours of *not* coding or exercising.

# Summary Table

| Technique | Complexity | Primary Domain | Core Question |
| :--- | :--- | :--- | :--- |
| **Decomposition** | High | Engineering/Physics | What is this made of? |
| **Quantitative** | High | Finance/Math | What do the numbers say? |
| **Root Cause** | Medium | Debugging/Maintenance | Why did this break? |
| **Systems** | Medium | Robotics/Strategy | How do parts interact? |
| **Inversion** | Low | Daily Habits | How do I guarantee failure? |
| **Second-Order** | Low | Decision Making | What happens *after* the result? |
