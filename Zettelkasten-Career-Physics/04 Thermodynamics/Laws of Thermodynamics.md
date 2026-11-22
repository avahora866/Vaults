---
tags:
  - BRANCH
  - REVIEW_NEEDED
---

# Laws of Thermodynamics

Thermodynamics rests on a small number of laws, each capturing an empirical regularity so consistent it's treated as inviolable.

## Zeroth law (implicit foundation)

If two systems are each in thermal equilibrium with a third, they are in thermal equilibrium with each other. This is what makes the concept of **temperature** well-defined and measurable — it justifies using a thermometer.

## First law — conservation of energy

$$
\Delta U = Q - W
$$

The change in a system's internal energy $\Delta U$ equals the heat $Q$ added to it minus the work $W$ done *by* it on its surroundings. This is simply energy conservation, stated in terms relevant to heat engines: you can't get more work out of a system than the energy you put in as heat, and any energy that goes in has to show up somewhere (as increased internal energy, or as work done).

## Second law — entropy and irreversibility

Heat spontaneously flows from hot objects to cold ones, never the reverse, unless external work is done (as in a refrigerator). More generally: the total **entropy** of an isolated system never decreases over time. Entropy is, loosely, a measure of disorder or of the number of equivalent microscopic ways a system's macroscopic state could be arranged — see [[Entropy and Irreversibility]].

This law has a profound consequence: it's why processes have a preferred direction in time (an egg scrambles but doesn't unscramble), even though the underlying microscopic laws of motion (Newton's, or the Schrödinger equation) are time-reversible. It also sets a fundamental limit on the efficiency of any heat engine — no engine operating between two temperatures can be perfectly efficient, because some energy must always be "wasted" as low-grade heat to satisfy the second law.

## Third law (brief mention)

As a system's temperature approaches absolute zero, its entropy approaches a minimum constant value — in practice, this means absolute zero can be approached arbitrarily closely but never actually reached.

## Related
* [[02 Thermodynamics]] (parent)
* [[Entropy and Irreversibility]]
* [[Statistical Mechanics]]
