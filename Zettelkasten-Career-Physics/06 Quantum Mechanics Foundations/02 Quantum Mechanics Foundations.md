---
tags:
  - TRUNK
  - REVIEW_NEEDED
---

# Quantum Mechanics Foundations

**Quantum mechanics** is the physical theory describing matter and energy at the smallest scales — atoms, electrons, photons — where the classical pictures built up in [[02 Mechanics|Mechanics]], [[02 Waves and Oscillations|Waves]], and [[02 Electromagnetism|Electromagnetism]] stop giving accurate predictions. It grew directly out of the puzzles covered in [[02 Modern Physics]] — quantized energy, wave-particle duality — and resolves them within a single, mathematically precise framework.

## Quantum states

A quantum system's state is described by a **wavefunction** (or more abstractly, a **state vector**), which unlike a classical description doesn't pin down definite values for every property at once. Instead it can exist in a **superposition** — a combination — of multiple possible outcomes simultaneously, in direct analogy to how classical waves can be superposed (see [[Superposition and Interference]]). Two or more quantum systems can also become **entangled**, correlated in a way that has no classical analogue. See [[Quantum States]].

## Measurement

Quantum mechanics doesn't predict definite outcomes for most measurements — it predicts *probabilities* for each possible outcome. Measuring a system forces it to "choose" one definite outcome from its superposition, with a probability set by the state's mathematical description (its wavefunction). This is fundamentally different from classical uncertainty (where we're just ignorant of a definite, pre-existing value) — quantum indeterminacy appears to be a genuine feature of nature, not a gap in our knowledge. See [[Measurement and Observables]] and [[Heisenberg Uncertainty Principle]].

## State evolution: the Schrödinger equation

Between measurements, a quantum state evolves smoothly and deterministically according to the **Schrödinger equation**,

$$
i\hbar \frac{\partial}{\partial t}\Psi = \hat{H}\Psi
$$

the central equation of non-relativistic quantum mechanics — playing a role analogous to Newton's second law in classical mechanics, but governing the evolution of the wavefunction rather than a particle's trajectory. Solving it for different physical situations (a particle in a box, an atom, a harmonic oscillator) reproduces the quantized energy levels seen in [[Energy Quantization]] as a natural mathematical consequence, rather than an assumption bolted on. See [[Schrodinger Equation and State Evolution]].

## Two illustrative systems

Two simple systems recur constantly as worked examples and building blocks throughout quantum mechanics: the **quantum harmonic oscillator** (the quantum version of the spring-mass system from [[Simple Harmonic Motion]]) and **two-level systems** (any quantum system with just two relevant states, like a spin-½ particle or a photon's polarization). See [[Quantum Harmonic Oscillator]] and [[Two-Level Systems]].

## Scope note

This trunk and its branches deliberately stop at the physical foundations of quantum mechanics. The engineering layer built on top — encoding information in qubits, quantum logic gates, quantum circuits and algorithms, and physical quantum hardware — is a distinct, applied topic that builds on everything here but goes beyond the scope of this pass through the vault.

## See also
* [[02 Modern Physics]] — the historical puzzles (quantization, wave-particle duality) this theory resolves.
* [[02 Waves and Oscillations]] — the superposition and interference mathematics reused here for wavefunctions.
* [[Simple Harmonic Motion]] — the classical system whose quantum version is the quantum harmonic oscillator.
