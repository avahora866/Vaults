---
tags:
  - LEAF
  - REVIEW_NEEDED
---

# Work-Energy Theorem

The **work-energy theorem** states that the net work done on an object equals the change in its kinetic energy:

$$
W_{net} = \Delta KE = \tfrac{1}{2}mv_f^2 - \tfrac{1}{2}mv_i^2
$$

This follows directly from Newton's second law: integrating $F = ma$ over distance, rather than solving for acceleration over time, lands you on this energy relationship.

**Why it's useful**: often you care about an object's speed at some point, but the force acting on it varies in a complicated way (a spring, air resistance, a curved incline). Rather than solving a differential equation for the motion, you can compute the total work done — sometimes just an area under a force-vs-distance graph — and get the final kinetic energy directly, without ever finding the detailed acceleration profile.

This theorem is a special case of the broader idea of energy conservation: it accounts only for kinetic energy and the net work done by *all* forces (conservative and non-conservative alike). If you instead want to track kinetic and potential energy separately, you use the conservation of mechanical energy from [[Energy and Work]], which folds conservative forces into potential energy terms and leaves only the work done by non-conservative forces (like friction) on the right-hand side.

## Related
* [[Energy and Work]] (parent)
* [[Newtons Laws of Motion]]
