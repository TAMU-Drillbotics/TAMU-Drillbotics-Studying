# Chapter 4: Examples — Layer 1 Knowledge Map

> Source: Åström & Murray, *Feedback Systems*, 2nd ed., Chapter 4, pp. 4-1 to 4-30.
> Layer: 1 (knowledge map).
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It is a **catalog of worked models** from many fields. The book reuses them in later chapters and in the exercises. [Stated, chapter intro] It introduces almost no new theory. Its job is to give you **concrete systems** on which to practice the ideas of *state, input, output, and dynamics* from Ch. 3.

### What problems does it address?
- How do you turn a real system (car, bicycle, circuit, web server, microscope, the human body, a population) into a state space model?
- What simplifying assumptions are made, and when do they stop being valid? The chapter quote says: "Don't believe that the model is the reality." [Stated, chapter epigraph]

### How the book says to use it
"First-time readers may wish to focus on only a few examples with which they have had the most prior experience." [Stated, chapter intro] **Treat this chapter as a reference.** Read it in depth only when a later chapter calls on an example. [Inferred: a recommendation]

### Major components (one per section)
1. Cruise control (§4.1)
2. Bicycle dynamics (§4.2)
3. Operational amplifier circuits (§4.3)
4. Computing systems and networks (§4.4)
5. Atomic force microscopy (§4.5)
6. Drug administration (§4.6)
7. Population dynamics (§4.7)

### After this chapter you should be able to:
- For any of the seven examples, name the **state, input, output, disturbance**, and the **main simplifying assumptions**.
- Explain *where the feedback is* in each example. In two cases (the bicycle's front fork and the op amp) the feedback is hidden in the physical design.
- Recognize each example when a later chapter refers back to it.

---

## 2. Knowledge Structure

> For each example: what it models, which **concepts it illustrates**, and **where it comes back**. "Comes back in" comes from a keyword search of the later chapters. It shows where the example is reused, not the full depth of the reuse. [Inferred]

### Module A — Cruise control (§4.1) ★ most-used example
**Purpose:** A simple, familiar regulation problem.

| Item | Content |
|---|---|
| Model | Force balance on the car: engine torque (a function of speed and gear), aerodynamic drag, rolling friction, gravity on a slope (the disturbance). Nonlinear 1st-order ODE (eq. 4.3). [Stated §4.1] |
| Controller | PI controller. [Stated §4.1] |
| Concepts illustrated | Disturbance rejection (hill), robustness to mass change, integral action driving the error to zero, model validity range, **feedback lets a crude model be good enough**. [Stated §4.1] |
| Comes back in | Ch. 6 (linearization), 7, 9, 11 (PI design, windup), 13 (robustness), 15 |

**Prerequisites:** Ch. 3 state space; Newton's 2nd law; Ch. 1–2 PI idea.

---

### Module B — Bicycle dynamics (§4.2)
**Purpose:** A system where **feedback is built into the mechanical design**.

| Item | Content |
|---|---|
| Model | Simple roll-dynamics model of the frame (4.5) plus an empirical front-fork model (4.6). Whipple model (4.8) for more accuracy. [Stated §4.2] |
| Concepts illustrated | **Hidden feedback**: the front fork with positive *trail* creates feedback from roll angle to steering angle. The bike is **unstable at low speed** and **self-stabilizing above a critical speed**. Velocity is a parameter that changes stability. [Stated §4.2] |
| Comes back in | Ch. 5 (stability vs parameters), 7, 8, 14 (fundamental limits; the rear-wheel-steered bicycle), 15 |

**Prerequisites:** Ch. 3 balance systems (Ex. 3.2–3.3); rigid body mechanics (angles, torques).

---

### Module C — Operational amplifier circuits (§4.3)
**Purpose:** Black's feedback amplifier in modern form.

| Item | Content |
|---|---|
| Model | Static model (zero input currents, output = gain × input difference) → dynamic model with a **gain-bandwidth product**. [Stated §4.3] |
| Concepts illustrated | **Trading high gain for robustness and linearity**; *loop gain*; sensitivity reduction; why block diagrams are not always a good fit for physical circuits; op amps as building blocks for analog controllers (PI) and analog computing. [Stated §4.3] |
| Comes back in | Ch. 6, 9 (transfer functions of circuits), 10, 11, 13, 15 |

**Prerequisites:** Ch. 2 §2.5 (feedback amplifier); basic circuit laws (Ohm, Kirchhoff).

---

### Module D — Computing systems and networks (§4.4)
**Purpose:** Show feedback in software and networks.

| Item | Content |
|---|---|
| Sensors and actuators | Sensors = resource utilization (CPU load, memory, bandwidth). Actuators = limits on resources (e.g., MaxUsers). [Stated §4.4] |
| Sub-examples | **Web server control** (keep CPU/memory utilization at target levels); **congestion control** (TCP-like window adjustment by routers and sources). [Stated §4.4] |
| Concepts illustrated | Feedback without physics; discrete-time and queuing models; large decentralized networks. [Inferred] |
| Comes back in | Ch. 5, 7, 10 (time delays in congestion control), 12, 15 |

**Prerequisites:** Ch. 3 difference equations and queuing (Ex. 3.5, 3.15).

---

### Module E — Atomic force microscopy (§4.5)
**Purpose:** A precision instrument that needs fast, accurate feedback at the nanometer scale.

| Item | Content |
|---|---|
| Model | Cantilever (a spring–mass-type system) plus piezo actuator plus sensor. Tapping and contact modes. [Stated §4.5] |
| Concepts illustrated | High-performance tracking; mechanical resonances; actuator dynamics as a limit on performance. [Inferred from later use] |
| Comes back in | Ch. 6, 9, 10, 11, 12 (loop shaping), 13, 15 |

**Prerequisites:** Ch. 3 spring–mass system.

---

### Module F — Drug administration (§4.6)
**Purpose:** A biomedical control problem, often run open loop.

| Item | Content |
|---|---|
| Sub-models | **Compartment models** (drug spreads in compartments and is removed at a rate proportional to concentration); **insulin–glucose dynamics**. [Stated §4.6] |
| Concepts illustrated | "Take two pills three times a day" = **open loop control** that is quantized and sampled. Linear compartment (state space) models. Glucose regulation as natural feedback. [Stated §4.6] |
| Comes back in | Ch. 5, 6, 7, **8 (heavily: state estimation)**, 15 |

**Prerequisites:** Ch. 3 state space; the idea of a concentration as a state variable.

---

### Module G — Population dynamics (§4.7)
**Purpose:** Nonlinear ecological models.

| Item | Content |
|---|---|
| Sub-models | **Logistic growth** (growth limited by carrying capacity); **predator–prey** (continuous-time version of Ex. 3.4). [Stated §4.7] |
| Concepts illustrated | Nonlinear dynamics, equilibria, oscillations (limit cycles). [Inferred from Ch. 5 use] |
| Comes back in | Ch. 5 (equilibria, limit cycles, stability), 6, 7 |

**Prerequisites:** Ch. 3 (Ex. 3.4 predator–prey).

---

## 3. Dependency Map

All seven modules depend on **Ch. 3 state space modeling** and are **independent of each other**. There is no required order inside this chapter. [Stated: "focus on only a few examples"; that they are mutually independent is Inferred]

```
                Ch. 3: state, input, output, dx/dt = f(x,u), y = h(x,u)
                                       │
   ┌──────────┬──────────┬────────────┼─────────────┬───────────┬───────────┐
   ▼          ▼          ▼            ▼             ▼           ▼           ▼
 4.1 Cruise 4.2 Bicycle 4.3 Op amp  4.4 Computing 4.5 AFM   4.6 Drugs   4.7 Population
 (needs     (needs Ex.  (needs      (needs        (needs    (needs      (needs
  PI idea,   3.2–3.3,    Ch. 2 §2.5, difference    spring–   compartment Ex. 3.4)
  Newton)    mechanics)  circuits)   equations)    mass)     idea)
```

**Recommended order for a control engineering student (Drillbotics focus) [Inferred]:**
1. **4.1 Cruise control** (essential, used everywhere, the template for PI speed control)
2. **4.3 Op amp** (essential for understanding *loop gain* and robustness)
3. **4.2 Bicycle** (good intuition about stability and hidden feedback)
4. **4.5 AFM** (resonant mechanical systems, closest in spirit to a drillstring)
5. 4.6, 4.4, 4.7: read when a later chapter refers to them.

**Depend on several earlier concepts:**
- Op amp (4.3) needs Ch. 2's feedback amplifier **and** circuit laws.
- Bicycle (4.2) needs balance systems (Ch. 3) **and** the idea that feedback can stabilize (Ch. 1 §1.5).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Cruise control model and PI loop; the general lesson that a simple model plus feedback is often enough; op amp as feedback amplifier (loop gain, trading gain for robustness) |
| **Supporting** | Bicycle front fork as hidden feedback and speed-dependent stability; AFM as a resonant precision positioning system; compartment models; web server control (sensors and actuators in software) |
| **Advanced / postpone** | Whipple bicycle model; gain-bandwidth product details; congestion control equations; insulin–glucose model; logistic and predator–prey models (needed only when Ch. 5 uses them) |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Newtonian mechanics** (forces, torques, rotational inertia) for 4.1, 4.2, 4.5.
- **Circuit theory** (Ohm's law, Kirchhoff's laws, ideal op amp rules) for 4.3.
- **Basic queuing ideas** (arrival rate, service rate) for 4.4.
- **Basic pharmacokinetics / biology** for 4.6. The book explains enough.

### Terms that need clarification
| Term | Where it appears |
|---|---|
| Trail (bicycle) | §4.2: the distance between the steering-axis ground point and the wheel contact point. It creates the self-stabilizing feedback. |
| Gain-bandwidth product | §4.3: an op-amp parameter with units of frequency. It links gain and speed. |
| Loop gain | §4.3 uses it before its formal definition in Ch. 10 (loop transfer function). |
| Compartment | §4.6: a well-mixed volume with a uniform concentration. |
| Linearization | Several examples are linearized "around an operating point". The formal method is in §6.4. |

### Topics the chapter does not explain enough
- **How the models were validated.** The book warns about model limits but gives few validation procedures.
- **Parameter values.** Many come from the literature without derivation.
- **Controller design** for these examples is *not* done here. It is spread across Ch. 5–15.
- The **AFM** and **congestion control** models use physics and networking background the book only summarizes.

### Note for Drillbotics [Inferred]
The team plan's WOB/RPM control is structurally closest to **cruise control** (regulate a speed against a load disturbance). The drillstring's torsional dynamics (stick-slip) are closest to the **AFM's resonant mechanics**. Use those two as mental templates when you build the Simscape model.
