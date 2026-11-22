---
tags:
  - BRANCH
  - REVIEW_NEEDED
---

# Schrödinger Equation and State Evolution

Between measurements, a quantum system's state doesn't jump around randomly — it evolves smoothly and completely deterministically, governed by the **Schrödinger equation**:

$$
i\hbar \frac{\partial}{\partial t}\Psi(x,t) = \hat{H}\Psi(x,t)
$$

Here $\Psi$ is the wavefunction, $\hbar$ the reduced Planck constant, and $\hat{H}$ the **Hamiltonian operator**, representing the total energy of the system (kinetic plus potential, generalised into operator form). This equation plays a role for quantum mechanics analogous to $F=ma$ for classical mechanics: given a system's Hamiltonian and its state at one instant, the Schrödinger equation determines its state at every later instant — right up until a measurement is made, at which point the separate, probabilistic measurement postulate takes over (see [[Measurement and Observables]]).

## The time-independent version and quantized energy

For systems where the Hamiltonian doesn't depend on time, the equation splits into a simpler, time-independent form:

$$
\hat{H}\psi = E\psi
$$

This is an **eigenvalue equation**: solving it for a given physical situation (a particle confined to a box, an electron in a hydrogen atom, a harmonic oscillator potential) yields a specific, discrete set of allowed energies $E$ and corresponding wavefunctions $\psi$ — exactly reproducing, from first principles, the kind of energy quantization that Bohr had to assume by hand (see [[Bohr Model of the Atom]]). This is one of the most important payoffs of the theory: quantization isn't a separate assumption bolted onto quantum mechanics, it *falls out* of solving this one equation for a confined system, in much the same way only certain wavelengths fit as [[Standing Waves|standing waves]] on a fixed string.

## Two exactly solvable examples

Very few physical situations can be solved for exactly by hand, but two recur constantly as reference points: the **quantum harmonic oscillator** (see [[Quantum Harmonic Oscillator]]) and simple **two-level systems** (see [[Two-Level Systems]]).

## Related
* [[02 Quantum Mechanics Foundations]] (parent)
* [[Measurement and Observables]]
* [[Bohr Model of the Atom]]
* [[Quantum Harmonic Oscillator]]
