---
tags:
  - BRANCH
  - REVIEW_NEEDED
---

# Statistical Mechanics

**Statistical mechanics** provides the microscopic foundation for thermodynamics: it explains bulk, measurable quantities like temperature, pressure, and entropy as emergent statistical properties of enormous numbers of individual particles (typically on the order of $10^{23}$, Avogadro's-number-scale), rather than as fundamental quantities in their own right.

## Temperature, microscopically

**Temperature** is directly related to the average kinetic energy of particles in a system. For an ideal gas, the average translational kinetic energy per particle is

$$
\langle KE \rangle = \tfrac{3}{2}k_B T
$$

where $k_B$ is Boltzmann's constant. Higher temperature simply means particles are, on average, moving faster.

## Entropy, microscopically

As covered in [[Entropy and Irreversibility]], entropy counts the number of microscopic arrangements consistent with a given macroscopic description. This reframes the second law of thermodynamics as a statement of overwhelming statistical likelihood rather than an additional mechanical law.

## Distributions

Rather than tracking every particle's exact state, statistical mechanics works with **probability distributions** over particle energies — the Maxwell-Boltzmann distribution for classical particles describes the spread of speeds in a gas at a given temperature, for instance. This probabilistic, "track the distribution, not the individual" approach is philosophically (and often mathematically) similar to the probabilistic description used in [[02 Quantum Mechanics Foundations|quantum mechanics]], though for entirely different reasons — statistical mechanics uses probability because tracking every particle is impractical; quantum mechanics uses probability because outcomes are, as far as we know, fundamentally indeterminate.

## Why this matters for advanced quantum topics

Real quantum systems (e.g. a qubit sitting in a real, imperfectly isolated device) are never perfectly isolated from their environment. Understanding how such a system exchanges energy with a large statistical "bath" of surrounding degrees of freedom — the domain of quantum statistical mechanics — is essential for understanding **decoherence** and noise in real quantum hardware.

## Related
* [[02 Thermodynamics]] (parent)
* [[Entropy and Irreversibility]]
* [[Laws of Thermodynamics]]
