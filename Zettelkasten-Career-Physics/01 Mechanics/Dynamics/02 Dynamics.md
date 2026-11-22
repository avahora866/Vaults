---
tags:
  - TRUNK
  - REVIEW_NEEDED
---

# Dynamics

**Dynamics** is the branch of mechanics that explains motion in terms of its causes — forces, momentum, and energy — building on the descriptive vocabulary of [[02 Kinematics|kinematics]]. Where kinematics asks *what* the motion looks like, dynamics asks *why*: what forces are acting, and what motion do they produce?

## Newton's Laws of Motion

Isaac Newton's three laws are the foundation of classical dynamics:

1. **First law (inertia)**: an object stays at rest or moves at constant velocity unless acted on by a net external force.
2. **Second law**: force equals mass times acceleration,
$$
\vec{F} = m\vec{a}
$$
This single equation is the workhorse of classical mechanics — given the forces on an object, it tells you exactly how it will accelerate.
3. **Third law**: for every action there is an equal and opposite reaction — forces always come in pairs acting on two different bodies.

These laws also give rise to the concept of **momentum**, $\vec{p} = m\vec{v}$, and its conservation in isolated systems. See [[Newtons Laws of Motion]] for the full treatment, and [[Momentum and Impulse]] for how force relates to momentum change over time.

## Energy and work

Energy is the capacity to do work, and it comes in many interchangeable forms. **Kinetic energy** is the energy of motion,
$$
KE = \tfrac{1}{2}mv^2
$$
**Potential energy** is stored energy due to position or configuration — for gravity near Earth's surface, $PE = mgh$. The **work-energy theorem** connects the two: the net work done on an object equals its change in kinetic energy. In a closed system with only conservative forces (like gravity), total mechanical energy — kinetic plus potential — is conserved: it changes form but its sum stays constant. See [[Energy and Work]] for the details, and [[Power]] for how energy relates to *rate* of energy transfer.

## Harmonic motion

Many physical systems — a mass on a spring, a pendulum, an atom vibrating in a crystal lattice — oscillate back and forth around an equilibrium point when displaced. When the restoring force is proportional to displacement ($F = -kx$, Hooke's law), the result is **simple harmonic motion (SHM)**, whose solutions are sines and cosines with a characteristic frequency. This pattern reappears throughout physics: in [[02 Waves and Oscillations|waves]], in AC circuits, and — reinterpreted quantum mechanically — as the [[Quantum Harmonic Oscillator]], one of the few quantum systems solvable exactly by hand. See [[Simple Harmonic Motion]].

## Why this matters beyond physics

Dynamics is the direct ancestor of engineering statics, dynamics, and control theory. Robotics in particular leans on Newton's second law (computing the forces needed to produce a desired acceleration), energy methods (for stability and efficiency), and harmonic motion (vibration, resonance, and oscillatory control systems).

## See also
* [[02 Kinematics]] — the descriptive vocabulary dynamics builds on.
* [[02 Waves and Oscillations]] — oscillation generalised into wave propagation.
* [[02 Quantum Mechanics Foundations]] — where energy and momentum become quantized.
