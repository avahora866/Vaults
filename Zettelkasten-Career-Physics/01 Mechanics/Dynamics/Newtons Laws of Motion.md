---
tags:
  - BRANCH
---

# Newton's Laws of Motion

Newton's three laws describe the relationship between a body's motion and the forces acting on it. Together they define **dynamics**: the study of motion caused by forces, as opposed to kinematics, which merely describes motion.

## First Law — Inertia

> An object at rest stays at rest, and an object in motion stays in motion at constant velocity, unless acted on by a net external force.

This defines **inertial reference frames** and introduces **inertia**: an object's inherent resistance to a change in its state of motion. Inertia is quantified by mass. See [[Inertia and Mass]].

## Second Law — F = ma

> The net force on an object equals its mass times its acceleration.

$$
\vec{F}_{net} = m\vec{a}
$$

This is a vector equation: force and acceleration point in the same direction. It is also, equivalently, a statement about momentum:

$$
\vec{F}_{net} = \frac{d\vec{p}}{dt}, \qquad \vec{p} = m\vec{v}
$$

For constant mass these two forms are identical, but the momentum form is the more general one — it still works when mass itself changes (e.g. a rocket burning fuel). See [[Momentum and Impulse]].

## Third Law — Action and Reaction

> For every force one object exerts on a second, the second exerts an equal and opposite force back on the first.

Crucially, these two forces act on **different bodies**, so they never cancel each other out for a single object — they only balance when you consider the whole interacting system. This is why walking is possible: your foot pushes backward on the ground, and the ground pushes you forward.

## Putting it together

Solving a mechanics problem with Newton's laws usually means: (1) identify every force acting on the object (gravity, normal force, friction, tension, applied force — see [[Contact and Field Forces]] for what each of these actually is), (2) sum them as vectors to get $\vec{F}_{net}$, (3) apply $\vec{F}_{net} = m\vec{a}$ to find the resulting acceleration, then (4) use kinematics (see [[Kinematics Basics]]) to find velocity and position over time.

## Related
* [[02 Dynamics]] (parent)
* [[Kinematics Basics]]
* [[Inertia and Mass]]
* [[Momentum and Impulse]]
* [[Contact and Field Forces]]
* [[Energy and Work]]
