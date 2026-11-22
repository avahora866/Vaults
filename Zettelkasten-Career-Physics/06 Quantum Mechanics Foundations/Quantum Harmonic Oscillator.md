---
tags:
  - LEAF
  - REVIEW_NEEDED
---

# Quantum Harmonic Oscillator

The **quantum harmonic oscillator** is the quantum-mechanical version of the classical mass-on-a-spring system from [[Simple Harmonic Motion]] — a particle in a parabolic ("quadratic") potential well, $V(x) = \tfrac{1}{2}kx^2$. It's one of the few systems the Schrödinger equation can be solved for exactly, which is why it appears constantly as a reference model throughout quantum mechanics — for molecular vibrations, phonons in solids, and modes of the quantum electromagnetic field itself.

## Quantized, evenly spaced energy levels

Unlike the classical oscillator, which can have any energy depending on its amplitude, the quantum harmonic oscillator's allowed energies are discrete and evenly spaced:

$$
E_n = \left(n + \tfrac{1}{2}\right)\hbar\omega, \qquad n = 0, 1, 2, \dots
$$

where $\omega = \sqrt{k/m}$ is the classical angular frequency (same formula as [[Simple Harmonic Motion|classical SHM]]). Each step up in $n$ adds exactly one quantum of energy, $\hbar\omega$ — this evenly spaced ladder of energy levels is the direct quantum echo of Planck's original hypothesis that energy comes in discrete units of $hf$.

## Zero-point energy

Notably, even in its lowest possible state ($n=0$), the oscillator has energy $E_0 = \tfrac{1}{2}\hbar\omega$ — not zero. This **zero-point energy** is a direct consequence of the [[Heisenberg Uncertainty Principle|uncertainty principle]]: a particle confined near the bottom of the potential well can't have exactly zero position uncertainty *and* exactly zero momentum uncertainty simultaneously, so it can never be perfectly at rest at the bottom of the well.

## Related
* [[02 Quantum Mechanics Foundations]] (parent)
* [[Simple Harmonic Motion]]
* [[Heisenberg Uncertainty Principle]]
