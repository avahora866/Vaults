---
tags:
  - "#TRUNK"
  - ElectricalEngineering
  - CircuitTheory
---

# Electrical Engineering — Circuit Theory & Components

Electrical engineering begins not with complex machines, but with three invisible quantities: charge, current, and voltage. Master these, add the laws that govern how they behave in circuits, and you can understand — and eventually design — almost any electronic system. This note covers the full introductory curriculum: the fundamental quantities, Ohm's Law, power, Kirchhoff's circuit laws, and a systematic tour of every major component class.

---

# 1. The Trinity of Electricity

Before any circuit can be understood, three quantities must be mastered. Everything else in electrical engineering is built on top of them.

## 1.1 — Electric Charge (Q)

Matter is made of atoms. Atoms contain protons (positive), neutrons (neutral), and electrons (negative). In a stable atom these cancel out — net charge is zero. Electricity is almost entirely the story of **electrons**, because unlike protons (locked in the nucleus), electrons are light and mobile.

**Charge** is the fundamental property that causes electric force. Its symbol is **Q** and its unit is the **Coulomb (C)**.

> One Coulomb ≈ 6.242 × 10¹⁸ electrons. That is roughly 6.24 *quintillion* electrons — an enormous number. A single electron carries only −1.602 × 10⁻¹⁹ C.

*Water analogy:* Charge is the **volume of water** in a pipe system — the "stuff" that flows.

> ⚠️ **The Crucial Detail — Removing Electrons = Positive Charge:** If a neutral wire has electrons *removed*, the temptation is to call the result negative. This is wrong. Electrons *are* negative, so removing them removes negativity. The leftover unmatched protons give the wire a **net positive charge**. Removing 6.242 × 10¹⁸ electrons leaves a charge of **+1 Coulomb**.

---

## 1.2 — Electric Current (I)

Having charge is not enough — we need it to *move*. **Current** is the rate of flow of electric charge past a point.

- **Symbol:** I (from French *intensité de courant*)
- **Unit:** Ampere (A)
- **Formula:** `I = Q / t`

Where Q is charge in Coulombs and t is time in seconds. Therefore: **1 Ampere = 1 Coulomb per second**.

*Water analogy:* Current is the **flow rate** — litres per second past a point in the pipe.

**Conventional Current vs. Electron Flow**

This is a historical quirk every engineer must know:

| | Direction | Origin |
|---|---|---|
| **Electron Flow** | Negative (−) → Positive (+) | Physical reality |
| **Conventional Current** | Positive (+) → Negative (−) | Benjamin Franklin's assumption |

> **The Crucial Detail:** In all of electrical engineering, *Conventional Current* is used by default — even though electrons physically move the opposite way. Unless a problem explicitly says "electron flow," assume current goes from + to −.

---

## 1.3 — Voltage (V)

Charge won't move on its own through a flat pipe. Something must push it. **Voltage** (also called *Electric Potential Difference*) is that push — the measure of potential energy available to drive charge from one point to another.

- **Symbol:** V (or E for Electromotive Force)
- **Unit:** Volt (V)
- **Formula:** `V = W / Q`

Where W is energy in Joules and Q is charge in Coulombs. Therefore: **1 Volt = 1 Joule of energy per Coulomb of charge moved**.

*Water analogy:* Voltage is **water pressure** — or more precisely, the *height difference* between two tanks. A tall tank has high potential energy; opening the valve produces strong flow.

> ⚠️ **The Crucial Detail — Voltage is always a *difference*:** You cannot say "Point A has 5 Volts." You must say "Point A is 5 Volts *higher than* Point B." Two tanks at 100m and 90m produce exactly the same flow as tanks at 10m and 0m — the pressure difference is 10m in both cases. This also explains why a bird can sit safely on a high-voltage power line: both feet are at the same potential, so the difference across the bird is **0V**, and zero current flows.

**Common voltage sources:**
- **Batteries** — convert chemical energy into electrical potential
- **Generators** — convert mechanical motion (spinning magnets) into electrical potential

---

# 2. Resistance & Ohm's Law

The Trinity gives us push (Voltage) and flow (Current). The missing piece is *opposition*.

## 2.1 — Resistance (R)

Not all materials allow electrons to move freely. **Resistance** measures how strongly a material opposes current flow.

- **Symbol:** R
- **Unit:** Ohm (Ω)

| Material Class | Examples | Resistance |
|---|---|---|
| **Conductor** | Copper, Gold | Very Low — electrons move freely |
| **Insulator** | Rubber, Glass | Very High — electrons are blocked |
| **Semiconductor** | Silicon | Tunable — forms the basis of transistors |

*Water analogy:* Resistance is the **narrowness of the pipe**. A wide, smooth pipe = low resistance. A narrow, clogged pipe = high resistance. Keeping pressure (Voltage) constant and narrowing the pipe (raising Resistance) reduces flow (Current).

---

## 2.2 — Ohm's Law

In 1827, Georg Ohm discovered that Voltage, Current, and Resistance are mathematically locked:

```
V = I × R
```

Rearranged into three forms using the **Ohm's Law Triangle** (cover the variable you want):

| To Find | Formula | In Words |
|---|---|---|
| **Voltage** | V = I × R | Current × Resistance |
| **Current** | I = V / R | Voltage ÷ Resistance |
| **Resistance** | R = V / I | Voltage ÷ Current |

**The core relationship:** Current is *directly proportional* to Voltage and *inversely proportional* to Resistance.
- Double the voltage → double the current
- Double the resistance → halve the current

> ⚠️ **The Danger Zone — Short Circuits:** A short circuit is a connection with near-zero resistance. By Ohm's Law: I = V / R → as R approaches 0, current spikes toward infinity. In practice this means the wire heats instantly, often melting or starting a fire. Fuses and circuit breakers exist specifically to catch this.

*Worked example:* A 9V battery connected to a 3Ω resistor → I = 9/3 = **3A**. Swap for 1Ω → I = 9/1 = **9A** (current triples).

---

# 3. Power & Energy

Ohm's Law tells us *how* current flows. Power tells us *what that current is doing* — lighting a room, heating a coil, spinning a motor.

## 3.1 — Electrical Power (P)

**Power** is the rate at which electrical energy is converted into another form (heat, light, motion).

- **Symbol:** P
- **Unit:** Watt (W)
- **Base formula (Joule's Law):** `P = V × I`

Because V = IR, two derived formulas let you calculate P when you only have two of the three variables:

| Known Variables | Formula | Notes |
|---|---|---|
| I and R | `P = I²R` | Current is *squared* — doubling I quadruples heat dissipated |
| V and R | `P = V²/R` | — |

> ⚠️ **Anti-Pattern — P = V × R:** A common beginner error is multiplying Voltage directly by Resistance. This has no physical meaning. Always find Current first (I = V/R), then compute P = V × I. Alternatively use P = V²/R directly.

*Worked example:* A car headlight on a 12V battery with a 4Ω filament:
```
// 1. Find current
I = V / R = 12 / 4 = 3A

// 2. Find power
P = V × I = 12 × 3 = 36W
```
This is less than a standard 60W household bulb.

---

## 3.2 — Energy (E) vs. Power (P)

These are frequently confused. The distinction is critical for understanding electricity bills.

| Concept | What it measures | Unit | Analogy |
|---|---|---|---|
| **Power (P)** | Rate of energy use *right now* | Watts (W) | How fast you're running |
| **Energy (E)** | Total energy accumulated over time | Joules (J) or Watt-hours (Wh) | How far you ran in total |

```
Energy = Power × Time
```

A 100W lightbulb left on for 2 hours consumes **200 Wh** of energy. This is what the electric company bills you for — not the wattage of your devices, but total energy consumed over time.

---

# 4. Circuit Analysis: Kirchhoff's Laws

A single component obeying Ohm's Law is straightforward. Real circuits have dozens of interconnected components. Two laws by Gustav Kirchhoff (1845) make them analysable.

## 4.1 — Kirchhoff's Current Law (KCL)

*The Law of Conservation of Charge.*

KCL applies at a **node** — any junction where two or more wires meet.

> **The Law:** The sum of all currents entering a node equals the sum of all currents leaving it. Current cannot accumulate or vanish at a junction.

```
I_in = I_out
```

*Water analogy:* A T-junction in a pipe. If 10 L/s flows in and 4 L/s exits left, exactly 6 L/s must exit right. No water appears or disappears.

*Worked example:* 5A enters a node splitting into three branches. Wire A carries 1A, Wire B carries 2A → Wire C must carry **2A** (5 − 1 − 2 = 2). If Wires B and C connect to identical resistors, their currents must be identical — a direct consequence of KCL and **symmetry in parallel circuits**.

---

## 4.2 — Series vs. Parallel: Current Behaviour

KCL immediately explains the two fundamental circuit topologies:

| | Series | Parallel |
|---|---|---|
| **Topology** | Components end-to-end (chain) | Components side-by-side (ladder) |
| **Current** | Same through every component | Splits at each node |
| **Path count** | One | Multiple |
| **Analogy** | Single-lane road | Multi-lane highway |

---

## 4.3 — Kirchhoff's Voltage Law (KVL)

*The Law of Conservation of Energy.*

KVL applies around a closed **loop** — any path that starts and ends at the same point.

> **The Law:** The algebraic sum of all voltages around any closed loop equals zero. What the source supplies, the components consume — exactly.

```
ΣV = 0   (around any closed loop)
```

*Hiking analogy:* Imagine a circular trail:
1. **Start** at the trailhead (Ground, 0V)
2. **Climb** the battery hill — gain +12m elevation (Voltage rise)
3. **Descend** Resistor 1 — lose 4m (Voltage drop)
4. **Descend** Resistor 2 — lose 8m (Voltage drop)
5. **Back** at trailhead: +12 − 4 − 8 = **0**

You cannot end higher or lower than where you started on a circular path. The same is true of voltage around any closed circuit loop.

---

## 4.4 — Series vs. Parallel: Voltage Behaviour

Voltage behaves as the *exact inverse* of current across the two topologies:

| | Series | Parallel |
|---|---|---|
| **Voltage** | Splits across components | Same across every branch |
| **Formula** | V_total = V₁ + V₂ + V₃ | V_battery = V_branch1 = V_branch2 |
| **Example** | 9V ÷ 3 identical bulbs = 3V each (dim) | 9V across 3 parallel bulbs = 9V each (bright) |

**The Four Rules — Master Summary:**

| Property | Series | Parallel |
|---|---|---|
| **Current** | Same everywhere | Divides |
| **Voltage** | Divides | Same everywhere |

> ⚠️ **The Crucial Detail — Christmas Lights:** Old Christmas light strings are wired in series. When one bulb burns out it creates an **open circuit** — infinite resistance, zero current. Because there is only one path, *no* current flows anywhere and every bulb goes dark. The voltage from the supply is still present at the gap, but electrons cannot jump it. This is the defining failure mode of series circuits.

---

# 5. Component Taxonomy

Understanding Voltage and Current means little if you cannot identify the physical components that control them. Below is a systematic tour of every component class you will encounter in standard electronics work, grouped by function.

---

## Group A — Passives (Energy Managers)

Passive components cannot generate or amplify energy. They only consume, store, or release it.

### 5.1 — Resistor
- **Function:** Limits current; divides voltage; dissipates energy as heat
- **Unit:** Ohm (Ω)
- **Common use:** Protecting LEDs from overcurrent; setting gain; forming voltage dividers

### 5.2 — Capacitor (C)

Without a way to smooth electrical noise, sensitive circuits behave erratically when voltage fluctuates. The capacitor solves this.

- **Function:** Stores energy in an **electric field**. Charges when voltage rises; releases stored charge when voltage dips. It therefore *opposes changes in voltage*.
- **Unit:** Farad (F) — practical values are microfarads (µF) or nanofarads (nF)
- **Behaviour:** Blocks DC; passes AC

*Water analogy:* A tank with a rubber membrane inside. Incoming pressure stretches the membrane (stores energy) and pushes back when source pressure drops — smoothing the flow.

- **Common use:** Smoothing power supply ripple; camera flashes (rapid energy dump); audio filtering

> **The Crucial Detail:** When supply voltage spikes, a capacitor across the rails *absorbs* the excess. When voltage sags, it *releases* stored charge to compensate. This is why decoupling capacitors appear on virtually every PCB.

### 5.3 — Inductor (L)

Where the capacitor opposes voltage change, the inductor opposes its counterpart.

- **Function:** Stores energy in a **magnetic field**. Resists changes in *current*. Physically it is a coil of wire.
- **Unit:** Henry (H)

*Water analogy:* A heavy flywheel on a pipe. It resists starting (opposes current increase) and, once spinning, keeps water moving even after the pump stops (opposes current decrease).

- **Common use:** Noise filters; LC oscillators; electromagnets; transformer windings

### 5.4 — Transformer
- **Function:** Two inductors placed in close proximity. Energy transfers between them via a shared magnetic field, allowing voltage to be stepped up or down with no direct electrical connection between input and output.
- **Common use:** Stepping 10,000V from a power line down to the 230V entering your home

**Passive Components — Quick Reference**

| Component | Stores/Uses | Opposes Change In | Unit |
|---|---|---|---|
| Resistor | Dissipates as heat | — | Ω |
| Capacitor | Electric field | Voltage | F |
| Inductor | Magnetic field | Current | H |
| Transformer | Magnetic field (transfer) | — | — |

---

## Group B — Variable Passives (User Control)

### 5.5 — Potentiometer ("Pot")
- **Function:** A resistor with a sliding **wiper** that divides the resistive track into two parts, creating an adjustable voltage divider. Moving the wiper changes the ratio.
- **Common use:** Volume knobs; light dimmers; joysticks

*Analogy:* A faucet handle — turning it adjusts how much resistance the current faces.

---

## Group C — Semiconductors (The "Smart" Components)

Semiconductors (typically silicon) can change their electrical behaviour based on applied voltage or current. They are the foundation of all active electronics and computing.

### 5.6 — Diode
- **Function:** The one-way valve of electronics. Current flows in one direction (**Forward Bias**); it is blocked in the other (**Reverse Bias**).
- **Symbol:** An arrow pointing in the direction of allowed conventional current, with a bar indicating the blocked direction
- **Common use:** Converting AC to DC (rectification); protecting circuits against reversed battery connections

### 5.7 — LED (Light Emitting Diode)

An LED is a diode that releases energy as photons (light) when forward-biased.

> ⚠️ **Two Non-Negotiable Rules for LEDs:**
> 1. **Polarity:** Connect Anode (+) to positive, Cathode (−) to negative. Reversed, it will not light and may be damaged.
> 2. **Current Limiting Resistor:** LEDs have near-zero internal resistance. Without a series resistor, current is bounded only by the source — the LED burns out instantly. Always calculate and place a resistor in series.

### 5.8 — Transistor

Arguably the most important invention of the 20th century. Every processor, phone, and computer is built from billions of them.

- **Function:** An electronically controlled **switch** or **amplifier**. A small signal at the *control* terminal governs a much larger current between the other two terminals.

*Digital faucet analogy:* Your finger (the control signal) turns a valve handle with minimal effort. The city water supply (the power source) does the actual work of filling the bucket (the load). Your finger does not generate the water — it merely *allows* it to flow.

| Type | Control Method | Terminals | Common Use |
|---|---|---|---|
| **BJT** (Bipolar Junction Transistor) | Current at Base | Base, Collector, Emitter | Audio amplifiers; switching |
| **MOSFET** (Metal-Oxide-Semiconductor FET) | Voltage at Gate | Gate, Drain, Source | Logic; processors; power switching |

> ⚠️ **Common Misconception:** A transistor does not *generate* the large current — it does not create power from nothing (Conservation of Energy). It uses a weak signal to *switch* or *proportion* a larger current from a separate power source. A 0.02A microcontroller pin can safely switch a 10A motor by triggering the transistor's gate while the motor draws from its own supply.

---

## Group D — Integrated Circuits (ICs)

An IC is a complete functional block — resistors, capacitors, and millions of transistors — etched onto a single silicon die and packaged into a small chip.

| IC | Function | Common Use |
|---|---|---|
| **Op-Amp** | Amplifies the voltage difference between two inputs; performs mathematical operations (add, subtract, integrate) | Audio amplification; sensor signal boosting |
| **555 Timer** | Generates precise time delays or repeating pulses (oscillations) | Blinking LEDs; tone generation; timing circuits |
| **Logic Gates (AND, OR, NOT)** | Accept binary inputs (High/Low) and produce a logical output | Digital computation; safety interlocks |

> **AND Gate rule:** Output is HIGH only when *both* Input A **AND** Input B are HIGH. The foundation of all binary decision-making in hardware.

---

## Group E — Electromechanical (Moving Parts)

These components bridge the gap between electrical signals and physical movement.

### 5.9 — Switch

| Type | Behaviour |
|---|---|
| **SPST** (Single Pole Single Throw) | Simple open/close — On or Off |
| **SPDT** (Single Pole Double Throw) | Toggles between two circuits — like a hallway light switch |

### 5.10 — Relay
- **Function:** An electromagnetically operated switch. A small current through the coil generates a magnetic field that physically pulls a metal contact closed, completing a separate high-power circuit.
- **Key advantage:** There is *no electrical connection* between the control circuit and the switched circuit — only a magnetic link. This means a 5V microcontroller can safely switch a 230V mains device.
- **Characteristic:** Produces an audible *click* on activation.

### 5.11 — Solenoid
- **Function:** Converts electrical energy into **linear mechanical motion** via a movable metal plunger inside a coil.
- **Common use:** Electronic door locks; automatic valves; pinball machine flippers

---

## Group F — Protection (Safety First)

These components are designed to fail *sacrificially* — they break so that your circuit, your devices, and your home do not.

| Component | Mechanism | Reusable? |
|---|---|---|
| **Fuse** | A thin wire that melts ("blows") when current exceeds its rated limit, permanently breaking the circuit | ❌ Must be replaced |
| **Circuit Breaker** | A mechanical switch that "trips" open on overcurrent | ✅ Can be reset |

> **Rule of thumb:** Fuses are cheap and fast; use them for protecting individual components. Circuit breakers are used at the mains/panel level where resettability matters.

---

*End of Trunk — Electrical Engineering: Circuit Theory & Components*
