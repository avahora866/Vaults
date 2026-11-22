---
tags:
  - LEAF
---

# Contact and Field Forces

Solving a Newton's-laws problem starts with identifying every force acting on an object (see [[Newtons Laws of Motion]]). Those forces fall into two broad categories: **field forces**, which act at a distance with no contact required, and **contact forces**, which only exist between objects that are touching.

## Gravity

**Gravity** is the everyday example of a field force. Near Earth's surface it's well approximated as constant:

$$
F_g = mg
$$

More generally, between any two masses,

$$
F_g = \frac{GMm}{r^2}
$$

Gravity's strength depends on an object's **gravitational mass**, which is experimentally identical to its **inertial mass** — the mass that appears in $F = ma$. See [[Inertia and Mass]] for why that equivalence matters.

## Normal force

The **normal force** is a contact force a surface exerts perpendicular to itself, preventing objects from passing through it. It has no fixed formula — it takes on whatever value is needed to satisfy that constraint. This is why the normal force on a block changes on an incline (only the component of gravity perpendicular to the surface needs balancing) or in an accelerating elevator (it must also supply the extra force for the acceleration).

## Friction

**Friction** resists relative sliding between two surfaces in contact.

* **Kinetic friction** acts while surfaces are already sliding: $f_k = \mu_k N$, opposing the direction of relative motion.
* **Static friction** resists the *onset* of sliding: $f_s \le \mu_s N$. Static friction isn't a fixed value either — it matches whatever applied force would otherwise cause motion, up to that maximum. Once the applied force exceeds $\mu_s N$, the object starts to slide and kinetic friction takes over.

## Tension

**Tension** is the pulling force transmitted through a string, rope, or cable, directed along its length. For an idealized massless, inextensible string (the standard simplifying assumption), tension is uniform along the entire length — the same string can't pull harder on one end than the other.

## Applied force

**Applied force** is a catch-all for any other push or pull delivered through direct contact — a hand pushing a box, a rope being pulled by a person — that doesn't fit the categories above.

## Givens vs. unknowns

A useful pattern when setting up a problem: gravity and applied forces are usually *given* quantities, while normal force, friction, and tension are typically *unknowns* solved for using $\vec{F}_{net} = m\vec{a}$ together with the physical constraints of the situation (a surface can't be passed through, a string can't stretch, an object on a surface doesn't accelerate perpendicular to it).

## Related
* [[Newtons Laws of Motion]] (parent)
* [[Inertia and Mass]]
* [[02 Dynamics]]
