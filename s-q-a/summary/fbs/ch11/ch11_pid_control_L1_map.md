# Chapter 11: PID Control — Layer 1 Knowledge Map

> Source: Åström & Murray, *Feedback Systems*, 2nd ed., Chapter 11, pp. 11-1 to 11-26.
> Layer: 1 (knowledge map).
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **PID control**, "by far the most common way of using feedback in engineering systems". It covers the basic properties of P, I, and D action, how to choose the parameters (tuning), how to handle actuator saturation (integral windup), and how to implement PID controllers in practice. [Stated, chapter intro] An industrial survey quoted in the epigraph found that **97% of regulatory controllers are PID**, and the chapter notes that derivative action is used in only about 4% of controllers. [Stated, epigraph and §11.1]

### What problems does it address?
- What does each term (P, I, D) do, and how does PID compare with model-based (observer) controllers? (§11.1)
- How can a 3-parameter controller handle a complex, high-order process? (§11.2: low-order models)
- How do we tune PID with little modeling effort? (§11.3: Ziegler–Nichols, FOTD-based tuning, relay feedback)
- What goes wrong when the actuator saturates, and how do we fix it? (§11.4: windup, anti-windup)
- What practical details make a PID work on real hardware? (§11.5: derivative filtering, setpoint weighting, op-amp and computer implementation)

### Position in the book
This is the first design chapter of the "classical" half. [Stated, Preface 1st ed.] It pulls together P/I/D from §1.6, PI design from §2.3, integral action from §7.4, and margins from Ch. 10. [Stated §11.1 refers back to §1.6 and §8.5; the other links are Inferred]

### Major components
1. Basic control functions: P, I, D; error feedback vs setpoint weighting (§11.1) ★
2. Simple controllers for complex systems: designing from low-order models (§11.2)
3. PID tuning: Ziegler–Nichols, FOTD-based rules, relay auto-tuning (§11.3) ★
4. Integral windup and anti-windup; manual/automatic switching (§11.4) ★
5. Implementation: derivative filtering, setpoint weighting, op amps, computer implementation (§11.5) ★

### After this chapter you should be able to:
- Write the PID law in gain form (*k_p, k_i, k_d*) and time-constant form (*k_p, T_i, T_d*), and convert between them.
- Explain the effect of increasing *k_p*, *k_i*, *k_d* on a step response.
- Design a PI/PID controller for a 1st- or 2nd-order model by pole placement.
- Apply Ziegler–Nichols (step response and frequency response methods) and explain their weaknesses.
- Explain integral windup and implement anti-windup (back-calculation with tracking time constant *T_t*).
- Implement a discrete-time PID with a filtered derivative and setpoint weighting.

---

## 2. Knowledge Structure

### Module A — Basic control functions (§11.1) ★
**Purpose:** Define PID precisely and understand each term. [Stated §11.1]

| # | Concept | One-line gloss |
|---|---|---|
| 1 | PID law (eq. 11.1) | *u = k_p e + k_i ∫e dτ + k_d de/dt = k_p(e + (1/T_i)∫e dτ + T_d de/dt)*. [Stated §11.1] |
| 2 | Parameters | *k_p* proportional gain, *k_i* integral gain, *k_d* derivative gain; *T_i* integral time, *T_d* derivative time. Also the proportional band *PB = 100/k_p*. [Stated §11.1] |
| 3 | P = present, I = past, D = future | D predicts by **linear extrapolation** of the error; I accumulates past error. [Stated §11.1] |
| 4 | PID vs observer-based control | An observer predicts with a *model*; PID predicts with linear extrapolation; PI does not predict at all. [Stated §11.1] |
| 5 | Effect of each term (on *P = 1/(s+1)³*) | P speeds up the response but leaves an offset; I removes the offset but can cause oscillation; D adds damping. [Stated §11.1, Fig. 11.2] |
| 6 | Integral action ⇒ zero steady-state error | If the closed loop settles, the integrator input (the error) must be zero. [Stated §11.1] |
| 7 | Why D is rare | It amplifies measurement noise, and the prediction horizon is hard to choose. It is useful when noise is low (e.g., temperature control). [Stated §11.1] |
| 8 | Error feedback vs P and D acting on *y* only | Two configurations; the second avoids large jumps when the setpoint changes. [Stated §11.1, Fig. 11.1] |

**Example:** PD action in the retina (11.1).

**Prerequisites:** Ch. 1 §1.6 (PID preview); Ch. 2 §2.3 (PI on a first-order system); Ch. 9 (transfer function of PID: *k_p + k_i/s + k_d s*).

---

### Module B — Simple controllers for complex systems (§11.2)
**Purpose:** A PID has few parameters, so model-based design must use **low-order models**. [Stated §11.2]

| Concept | One-line gloss |
|---|---|
| Controller complexity ↔ model complexity | Ch. 7–8 methods give a controller as complex as the model, so PID requires a 1st- or 2nd-order model. [Stated §11.2] |
| Low-order model approximation | Any stable system can be approximated near the frequencies that matter by a simple model. [Stated §11.2] |
| PI/PID by pole placement | For 1st/2nd-order models, choose the gains so the closed loop has desired poles (ζ, ω₀). [Stated §11.2] |
| Integral control of AFM | Pure I control for a system with a fast resonance. [Stated §11.2, Ex. 11.2] |
| Cruise control PI design | PI tuned by pole placement on the linearized car model. [Stated §11.2, Ex. 11.3] |

**Prerequisites:** Module A; Ch. 7 (eigenvalue assignment idea); Ch. 6 (linearization).

---

### Module C — PID tuning (§11.3) ★
**Purpose:** Practical rules for setting the parameters with little modeling. [Stated §11.3]

| Concept | One-line gloss |
|---|---|
| Ziegler–Nichols step response method | Measure the step response; extract two parameters (*a*, τ) from the steepest tangent; look up the gains in a table. [Stated §11.3] |
| Ziegler–Nichols frequency response method | Raise the P gain until the loop oscillates; record the critical gain *k_c* and period *T_c*; look up the gains. [Stated §11.3] |
| Critical point interpretation | *(k_c, T_c)* corresponds to where the Nyquist curve crosses the negative real axis. [Stated §11.3] |
| Weakness of Z–N | Uses only two process parameters; it often gives poorly damped loops. [Stated §11.3] |
| FOTD model (first-order + time delay) | *P(s) = K e^{−sτ}/(1 + sT)*: three parameters, which give better tuning rules. [Stated §11.3] |
| Relay feedback (auto-tuning) | Replace the controller with a relay; the loop oscillates at about the critical frequency. This finds *(k_c, T_c)* safely and automatically. [Stated §11.3] |

**Example:** AFM tapping mode (11.4).

**Prerequisites:** Module A; Ch. 10 (Nyquist plot, critical point); Ch. 9 (time delay); Ch. 10 §10.5 (describing function, for why relay feedback works). [Inferred for the describing function link]

---

### Module D — Integral windup (§11.4) ★
**Purpose:** Handle actuator saturation, "an important feature of many feedback systems". [Stated, chapter intro]

| Concept | One-line gloss |
|---|---|
| Actuator saturation | The actuator has limits (valve fully open, motor at max speed). [Stated §11.4] |
| Loop is effectively broken while saturated | The actuator output stays at its limit regardless of the error. [Stated §11.4] |
| Integral windup | The integrator keeps accumulating error during saturation, so there is a large overshoot when the system comes out of saturation. [Stated §11.4] |
| Anti-windup by back-calculation (tracking) | Feed back the difference between the actuator output (from a model or measurement) and the controller output to the integrator, with tracking time constant *T_t*. [Stated §11.4, Fig. 11.11] |
| Manual control and tracking | Switch between automatic and manual modes without bumps by letting the controller track the actual input. [Stated §11.4] |
| Anti-windup for general controllers | The same idea applied to state space controllers with integral action (observer fed with the actual actuator signal). [Stated §11.4] |

**Examples:** cruise control on a steep hill without and with anti-windup (11.5, 11.6).

**Prerequisites:** Module A (integral action); Ch. 1 §1.6 (saturation); Ch. 8 §8.5 (for the general-controller version).

---

### Module E — Implementation (§11.5) ★
**Purpose:** The practical details that make PID work on real hardware. [Stated §11.5]

| Concept | One-line gloss |
|---|---|
| Filtering the derivative | An ideal derivative has high gain at high frequency, which amplifies noise. Use *k_d s/(1 + sT_f)* or filter the measurement. [Stated §11.5] |
| Setpoint weighting | *u = k_p(βr − y) + k_i∫(r − y) + k_d(γ dr/dt − dy/dt)*. β and γ shape the response to setpoint changes without changing disturbance rejection. [Stated §11.5] |
| 2DOF interpretation | Setpoint weighting is a simple two-degree-of-freedom controller. [Inferred; it links to §2.4, §8.5, §12.4] |
| Op-amp implementation | PI/PID built with feedback around op amps. [Stated §11.5] |
| Computer implementation | Periodic sampling, A/D → compute → D/A. Discretize P, I (forward/backward differences), and filtered D. Pseudo-code is given. [Stated §11.5] |
| Sampling-related issues | Choosing the sampling period; anti-aliasing filters; computational delay. [Inferred: these are covered briefly; the book's emphasis is on the discretization formulas] |

**Example:** cruise control with setpoint weighting (11.7).

**Prerequisites:** Modules A and D; Ch. 6 §6.3 (sampling); Ch. 3 Example 3.6 (difference approximation of PI); Ch. 4 §4.3 (op amps).

---

## 3. Dependency Map

```
Ch. 1 §1.6 (P, I, D idea)   Ch. 2 §2.3 (PI on 1st-order plant)   Ch. 9 (TF of PID)
             └────────────────────┬───────────────────────────────┘
                                  ▼
Module A: PID LAW, k_p/k_i/k_d ↔ k_p/T_i/T_d, effect of each term, integral ⇒ zero error
                                  │
          ┌───────────────────────┼───────────────────────────┐
          ▼                       ▼                           ▼
Module B: design from      Module C: TUNING              Module D: WINDUP
low-order models            Ziegler–Nichols (step/freq)   saturation → windup →
(pole placement;            needs Ch. 10 critical point   anti-windup (tracking T_t)
needs Ch. 7 idea)           → FOTD rules (needs delay)    → manual/auto switching
                            → relay auto-tuning           → general controllers (Ch. 8)
                                  │                           │
                                  └─────────────┬─────────────┘
                                                ▼
                         Module E: IMPLEMENTATION
                         derivative filter, setpoint weighting (2DOF),
                         op amps, computer (sampled) implementation
```

**Must be understood first:** the PID law and what each term does (Module A).

**Can be learned independently (after A):**
- Modules B (model-based design), C (empirical tuning), and D (windup) do not depend on each other.
- In Module E, derivative filtering and setpoint weighting can each be learned alone.

**Depend on several earlier concepts:**
- Z–N frequency method (C) needs PID (A) **and** the Nyquist critical point (Ch. 10).
- Relay auto-tuning (C) needs Z–N (C) **and** (for understanding) describing functions (Ch. 10 §10.5).
- Anti-windup for state space controllers (D) needs windup (D) **and** observers (Ch. 8).
- Computer implementation (E) needs the PID law (A), anti-windup (D), derivative filtering (E), **and** sampling (Ch. 6).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | PID law in both parameterizations; the role of P, I, D; integral action ⇒ zero steady-state error; derivative amplifies noise ⇒ filter it; actuator saturation ⇒ integral windup; anti-windup by back-calculation; discrete-time PID implementation |
| **Supporting** | PI/PID design by pole placement on low-order models; Z–N step and frequency methods; FOTD model; relay auto-tuning; setpoint weighting (β, γ); manual/automatic bumpless transfer |
| **Advanced / postpone** | Proportional band; op-amp PID circuits; anti-windup for general state space controllers; detailed FOTD tuning formulas; PD action in the retina |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Transfer functions** of P, I, D (*k_p, k_i/s, k_d s*) and of a closed loop (Ch. 9).
- **Gain/phase margins and the Nyquist critical point** (Ch. 10), for the Z–N frequency method and for judging tuning quality.
- **Discretization** (Ch. 3 Ex. 3.6, Ch. 6 §6.3), for computer implementation.
- **Step response specs** (Ch. 6), to evaluate tunings.

### Terms that need clarification
| Term | Issue |
|---|---|
| *k_i* vs *T_i* | *k_i = k_p/T_i*. A *larger* *T_i* means *less* integral action. This is easy to confuse. |
| *k_d* vs *T_d* | *k_d = k_p T_d*. |
| Proportional band | An industrial alternative to *k_p*: *PB = 100/k_p* (%). |
| Windup vs saturation | Saturation is the actuator limit. Windup is the *integrator's* bad reaction to it. |
| Tracking time constant *T_t* | The speed at which the integrator is "reset" during saturation. A rule of thumb is given in §11.4. |
| Setpoint weighting β, γ | β = 1, γ = 1 is pure error feedback. β = 0, γ = 0 means P and D act only on *y*. |
| Critical gain *k_c* / period *T_c* | Also called the ultimate gain and period (*K_u, T_u*) in other texts. |

### Topics the chapter does not explain enough
- **Modern tuning rules** (e.g., λ-tuning, SIMC, AMIGO) are mentioned only through references. The FOTD-based rules are sketched.
- **Choosing the sampling period** for digital PID is covered only briefly.
- **Cascade control** (inner/outer loops) appears in Ch. 1 §1.5 and Ch. 15, not here. [Inferred]
- **Robustness of a tuned PID**: the chapter gives tuning rules, while robustness checks (*M_s*, margins) are in Ch. 10, 12, 13.
- **Gain scheduling for PID** across operating points: discussed in Ch. 8 §8.5 for state feedback, not here.

### Note for Drillbotics [Inferred]
This is the **most directly useful chapter** for Task 4 of the team plan (closed-loop WOB/RPM control with limits):
- **Anti-windup (Module D) is mandatory.** The rig's motors and feed actuator *will* saturate, and the hardware limits enforced by the safety layer are exactly the saturation that causes windup.
- **Derivative filtering (Module E)** matters because WOB and torque measurements are noisy.
- **Setpoint weighting (Module E)** avoids WOB spikes when the operator or the autonomous logic changes the setpoint.
- **Relay auto-tuning (Module C)** is a safe way to tune on the real rig.
- **Manual/automatic bumpless transfer (Module D)** maps directly to the drill/slide/rotate mode switching in Stateflow.
