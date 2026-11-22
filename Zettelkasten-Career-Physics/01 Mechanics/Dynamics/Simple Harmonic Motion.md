---
tags:
  - BRANCH
  - REVIEW_NEEDED
---

# Simple Harmonic Motion

**Simple harmonic motion (SHM)** is the motion of any system where the restoring force is directly proportional to displacement from equilibrium, and points back toward it:

$$
F = -kx
$$

This is **Hooke's law**, most familiar from springs, but the same mathematical form shows up whenever a system is displaced slightly from a stable equilibrium — a pendulum at small angles, a vibrating molecule, an LC electrical circuit, even (reinterpreted quantum mechanically) a particle in a parabolic potential well.

## The equation of motion

Combining Hooke's law with Newton's second law ($F = ma = m\ddot{x}$) gives a differential equation:

$$
m\ddot{x} = -kx \quad\Longrightarrow\quad \ddot{x} + \omega^2 x = 0, \qquad \omega = \sqrt{\frac{k}{m}}
$$

whose solution is sinusoidal:

$$
x(t) = A\cos(\omega t + \phi)
$$

where $A$ is the **amplitude** (maximum displacement), $\omega$ the **angular frequency**, and $\phi$ a phase constant fixed by initial conditions. The period is $T = 2\pi/\omega$, independent of the amplitude — a small oscillation and a large one (of the same system) take the same time to complete a cycle.

## Energy in SHM

As the system oscillates, energy trades continuously between kinetic and potential (spring) energy — see [[Energy and Work]] — with the total,

$$
E = \tfrac{1}{2}kA^2
$$

staying constant in the absence of friction.

## Damping and resonance

Real oscillators lose energy to friction or resistance (**damping**), so oscillations decay over time unless driven. When a system is driven periodically at a frequency close to its own **natural frequency**, the amplitude of oscillation grows dramatically — this is **resonance**. See [[Resonance]].

## Why SHM matters beyond mechanics

SHM is one of the most-reused patterns in physics. The same equation governs classical spring-mass systems, and — with the classical variables replaced by a wavefunction — the **quantum harmonic oscillator**, one of the handful of quantum systems that can be solved exactly (see [[Quantum Harmonic Oscillator]]). It's also the starting point for understanding wave motion generally (see [[02 Waves and Oscillations]]), since a wave can be thought of as many coupled oscillators.

## Related
* [[02 Dynamics]] (parent)
* [[Resonance]]
* [[02 Waves and Oscillations]]
* [[Quantum Harmonic Oscillator]]
