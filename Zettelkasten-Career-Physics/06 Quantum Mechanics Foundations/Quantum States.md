---
tags:
  - BRANCH
  - REVIEW_NEEDED
---

# Quantum States

A quantum system's **state** encodes everything that can be known about it, described mathematically by a **wavefunction** $\Psi$ (for a particle in space) or, more abstractly and generally, by a **state vector** $|\psi\rangle$ in an abstract vector space (a Hilbert space).

## Superposition

Unlike a classical system, which at any instant has one definite value for each of its properties, a quantum system can exist in a **superposition** — a combination — of multiple distinct states at once:

$$
|\psi\rangle = \alpha|0\rangle + \beta|1\rangle
$$

where $|0\rangle$ and $|1\rangle$ represent two distinct, mutually exclusive outcomes (e.g. two different energy levels, or two orthogonal polarization directions — see [[Polarization]]), and $\alpha, \beta$ are complex numbers whose squared magnitudes give the probability of each outcome upon measurement ($|\alpha|^2 + |\beta|^2 = 1$). This isn't merely "we don't know which state it's in" — a system in superposition genuinely doesn't have a definite value for that property until measured, evidenced by interference effects (like the double-slit pattern, [[Superposition and Interference]]) that a simple ignorance-of-a-pre-existing-value picture cannot reproduce.

## Entanglement

Two or more quantum systems can become **entangled**: their combined state cannot be described as each system independently being in its own separate state, no matter how far apart they are physically. Measuring one entangled particle instantaneously determines the corresponding measurement outcome for its partner — a correlation stronger than any possible classical explanation allows (a fact proven experimentally via violations of *Bell inequalities*). Entanglement is one of the most distinctly non-classical features of quantum mechanics, with no analogue in ordinary wave superposition.

## Related
* [[02 Quantum Mechanics Foundations]] (parent)
* [[Measurement and Observables]]
* [[Superposition and Interference]]
* [[Polarization]]
