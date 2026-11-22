---
tags:
  - TRUNK
  - REVIEW_NEEDED
---

# Kinematics

**Kinematics** is the branch of mechanics that describes motion — position, velocity, acceleration — without asking what causes it. It's the vocabulary that Newton's laws (see [[02 Dynamics|Dynamics]]) are later written in: dynamics explains *why* an object accelerates, kinematics just describes *what* that acceleration produces.

## The core quantities

* **Position** $x(t)$ (or $\vec{r}(t)$ in more than one dimension): where an object is, as a function of time.
* **Velocity** $v = \dfrac{dx}{dt}$: the rate of change of position — a vector, since it has direction as well as magnitude (speed is just its magnitude).
* **Acceleration** $a = \dfrac{dv}{dt} = \dfrac{d^2x}{dt^2}$: the rate of change of velocity.

Each is the time-derivative of the one before it, so kinematics is really calculus applied to motion: given a position function, differentiation gives velocity and acceleration directly; given an acceleration function, integration recovers velocity and position (up to constants fixed by initial conditions). See [[Kinematics Basics]] for the standard constant-acceleration equations used throughout introductory mechanics.

## Motion in more than one dimension

In two or three dimensions, motion along each axis is independent — a projectile's horizontal and vertical motion can be analysed separately (horizontal: constant velocity; vertical: constant acceleration under gravity) and then recombined as vectors to get the full trajectory. This decomposition into independent components is the same idea underlying [[Orthoganol Components]].

## Frames of reference

Position, velocity, and even acceleration are only meaningful relative to a chosen reference frame. Velocities add (and subtract) between frames moving at constant relative velocity — this is why a ball thrown forward inside a moving train has a different speed measured from the platform than from inside the carriage. Kinematics stays valid in any *inertial* frame (one not itself accelerating); dynamics is where the choice of frame starts to matter for the forces you need to account for.

## Why this matters beyond physics

Kinematics is the direct language of trajectory planning, motion profiles, and sensor fusion in robotics and controls — before a system can decide what forces or torques to apply (dynamics), it needs a precise description of the motion it's trying to produce or is currently measuring. GPS and inertial navigation systems, for instance, are fundamentally kinematic: integrating measured acceleration to track position over time.

## See also
* [[02 Dynamics]] — the forces and energy that explain *why* motion described here happens.
* [[02 Waves and Oscillations]] — motion that repeats itself in time and space.
