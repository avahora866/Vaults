---
tags:
  - BRANCH
  - REVIEW_NEEDED
---

# Standing Waves

A **standing wave** forms when two waves of the same frequency and amplitude travel through the same medium in opposite directions — most commonly, a wave and its own reflection. Rather than appearing to travel, the resulting pattern oscillates in place: some points stay perpetually motionless while others oscillate with maximum amplitude.

Mathematically, superposing a rightward and leftward travelling wave of the same frequency,

$$
y_1 = A\sin(kx - \omega t), \qquad y_2 = A\sin(kx + \omega t)
$$

gives

$$
y_1 + y_2 = 2A\sin(kx)\cos(\omega t)
$$

— a spatial pattern $\sin(kx)$ whose amplitude simply oscillates in time via $\cos(\omega t)$, rather than a pattern that shifts position. See [[Nodes and Antinodes]] for what the fixed and maximally-oscillating points are called and why they occur where they do.

## Why standing waves matter

Standing waves only occur at specific frequencies determined by the boundary conditions of the system (e.g. a string fixed at both ends, an air column open or closed at each end) — these allowed frequencies are the system's **resonant** or **harmonic** frequencies, directly connected to [[Resonance]]. This is the physical basis for musical instruments: a guitar string or a column of air in a flute can only "ring" at certain discrete frequencies.

This same idea — that confining a wave to a finite region forces it into a discrete set of allowed patterns — is one of the clearest classical analogies for **energy quantization** in quantum mechanics: an electron's wavefunction confined within an atom is likewise restricted to a discrete set of standing-wave-like patterns, each corresponding to a specific allowed energy. See [[Energy Quantization]].

## Related
* [[02 Waves and Oscillations]] (parent)
* [[Nodes and Antinodes]]
* [[Energy Quantization]]
