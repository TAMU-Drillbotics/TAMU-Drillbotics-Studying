# Chapter 4: A First Analysis of Feedback — Layer 1 Knowledge Map

> Source: Franklin, Powell & Emami-Naeini, *Feedback Control of Dynamic Systems*, 7th ed., Chapter 4, pp. 180–233.
> Layer: 1 (knowledge map). What to learn and in what order, not full explanations.
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It derives **the basic equations shared by all three design methods** (Ch. 5–7) and uses them to compare open-loop and feedback control with respect to **stability, tracking, regulation, and sensitivity**. [Stated, Perspective] It assumes the plant and controller are linear, time-invariant, and single-input single-output (SISO). It then introduces **system type** (steady-state error to polynomial inputs) and the **PID controller**.

### What problems does it address?
- Exactly *what* does feedback improve, and what does it cost? (The Ch. 1 cruise-control story, now with dynamics.)
- How large is the steady-state error to a step, ramp, or parabola, and how do we make it zero?
- What do the P, I, and D terms each do, and how do we pick their values (tuning)?
- How can feedforward help feedback?

### Major components
1. The basic equations of control: *S*, *T*, and the four objectives (§4.1) ★
2. Steady-state error and system type (§4.2) ★
3. The PID controller and Ziegler–Nichols tuning (§4.3) ★
4. Feedforward control by plant model inversion (§4.4)
5. Optional/web: digital control intro (§4.5), sensitivity of time response (§4.6)

### After this chapter you should be able to:
- Write *Y*, *U*, and *E* for a unity-feedback loop with reference *R*, disturbance *W*, and sensor noise *V*, using *S* and *T*.
- Explain the four things feedback can do that open loop cannot, and the conflict caused by sensor noise.
- Determine a system's type and its error constant (*K*ₚ, *K*ᵥ, or *K*ₐ), and compute the steady-state error.
- Describe the effect of each PID term on error and stability.
- Apply a Ziegler–Nichols tuning table from a step response or from the stability limit.
- Add a DC-gain feedforward term to remove a steady-state tracking error.

---

## 2. Knowledge Structure

### Module A — The basic equations of control (§4.1) ★
**Purpose:** Set up the standard feedback loop and see feedback's effect on each objective.

Learn these in order:

| # | Concept | One-line gloss |
|---|---|---|
| 1 | Open-loop structure | *R* → controller *D*ₒₗ → plant *G* → *Y*. No path from output back to control. [Stated §4.1, Fig. 4.1] |
| 2 | Unity-feedback structure | Controller *D*꜀ acts on *E* = *R* − (measured *Y*). Inputs: reference *R*, plant disturbance *W*, sensor noise *V*. [Stated §4.1, Fig. 4.2] |
| 3 | Loop gain *GD*꜀ | The product around the loop. [Stated §4.1] |
| 4 | Sensitivity function *S* | *S* = 1/(1 + *GD*꜀). [Stated §4.1, eq. 4.12] |
| 5 | Complementary sensitivity *T* | *T* = *GD*꜀/(1 + *GD*꜀), also the closed-loop transfer function. [Stated §4.1, eq. 4.13] |
| 6 | The compact equations | *Y* = *TR* + *GSW* − *TV*; *E* = *SR* − *GSW* + *TV*. [Stated §4.1, eqs. 4.9–4.11] |
| 7 | *S* + *T* = 1 | Follows from the definitions (Problem 4.1). [Stated, Problem 4.1] |
| 8 | Stability (§4.1.1) | Closed-loop poles are roots of the characteristic equation 1 + *GD*꜀ = 0, so feedback can move poles, including stabilizing an unstable plant. [Stated §4.1.1] |
| 9 | Tracking (§4.1.2) | Open loop needs an exact plant inverse; feedback reduces tracking error where the loop gain is large. [Stated §4.1.2] |
| 10 | Regulation (§4.1.3) | Open loop is useless against disturbances; feedback reduces the disturbance effect by *S*. Sensor noise enters through *T*, so there is a **conflict**: high gain rejects *W* but passes *V*. [Stated §4.1.3, Summary] |
| 11 | Sensitivity (§4.1.4) | The sensitivity of *T* to a change in *G*: (d*T*/*T*)/(d*G*/*G*) = *S*. Large loop gain makes the closed loop insensitive to plant changes. [Stated §4.1.4] |

**Why together:** One set of equations, four interpretations.

**Prerequisites:** Ch. 3 (transfer functions, block-diagram reduction, characteristic equation, stability); Ch. 1 (the goals; cruise control).

**Key relationship [Stated, Summary]:** "Compared to open-loop control, feedback can be used to stabilize an otherwise unstable system, to reduce errors to plant disturbances, to improve the tracking of reference inputs, and to reduce the system's transfer function sensitivity to parameter variations."

---

### Module B — Steady-state error and system type (§4.2) ★
**Purpose:** A shorthand for how well a loop follows step, ramp, and parabolic inputs.

| Concept | One-line gloss |
|---|---|
| Polynomial test inputs | Step (degree 0), ramp (degree 1), parabola (degree 2): *r*(*t*) = *t*ᵏ/*k*!. [Stated §4.2] |
| System type *k* (reference) | Zero steady-state error to polynomials of degree **< *k***, finite nonzero error to degree *k*. For a stable unity-feedback loop, *k* = number of poles of *GD*꜀ at the origin (integrators). [Stated, Summary] |
| Error constants | Position *K*ₚ = *GD*꜀(0) (Type 0), velocity *K*ᵥ (Type 1), acceleration *K*ₐ (Type 2): *K*ₖ = lim *s*ᵏ*GD*꜀(*s*). Table 4.1 gives the errors. [Stated, Summary] |
| Type is robust | In unity feedback, type does not change when parameters change (only the error constant does). [Stated §4.2.1] |
| Type for disturbances | Defined the same way, using the error caused by polynomial disturbances. It depends on **where** the disturbance enters. [Stated §4.2.2, Review Q 4.7] |
| Stability vs error trade-off | Changes that improve stability (e.g., some feedback structures) can increase steady-state error. [Stated §4.2] |
| Truxal's formula | Velocity constant from closed-loop poles and zeros; web only (W4.2.2.1). [Stated §4.2] |

**Why together:** All describe the **steady state** (after transients die), using the FVT.

**Prerequisites:** Module A (*E* = *SR* − …); Ch. 3 Final Value Theorem and stability.

---

### Module C — PID control (§4.3) ★
**Purpose:** The almost universal controller, built from three simple terms. [Stated, Chapter Overview]

Learn these in order:

| # | Concept | One-line gloss |
|---|---|---|
| 1 | Proportional (P) | *u* = *k*ₚ*e*. Raising *k*ₚ reduces steady-state error but usually reduces damping and can destabilize. [Stated §4.3.1, Summary] |
| 2 | Integral (I) | *u* = *k*ᵢ∫*e*. Gives **zero steady-state error** to constant references and disturbances, robustly; may reduce stability. [Stated §4.3.2, Summary] |
| 3 | Derivative (D) | *u* = *k*_D ė. Adds damping, improves stability. Can be placed in the feedback path instead of the error path (Review Q 4.10). [Stated §4.3.3, Summary] |
| 4 | PI and PID | *D*꜀(*s*) = *k*ₚ + *k*ᵢ/*s* + *k*_D *s*, or the process-industry form *k*ₚ(1 + 1/(*T*ᵢ*s*) + *T*_D *s*). [Stated §4.3.4–4.3.5, Summary] |
| 5 | Effect on the characteristic equation | Each term changes the closed-loop characteristic polynomial, so gains can be chosen to place poles. [Stated, Preface and §4.3] |
| 6 | Process reaction curve | Many process step responses are S-shaped and fit *A e*^{−*s t*_d}/(τ*s* + 1): a first-order lag plus time delay. [Stated §4.3.6] |
| 7 | Ziegler–Nichols: decay-ratio method | Tune from the reaction curve for a quarter-decay transient (ζ ≈ 0.21, "low for many applications"). Table 4.2. [Stated §4.3.6] |
| 8 | Ziegler–Nichols: ultimate sensitivity method | Raise *k*ₚ until the loop oscillates at the stability limit; tune from that gain and period. Table 4.3. [Stated §4.3.6] |

**Why together:** P, I, D are the three building blocks; tuning chooses their weights.

**Prerequisites:** Module A (characteristic equation); Module B (why integral action removes error); Ch. 3 (second-order specs, Routh).

---

### Module D — Feedforward by plant model inversion (§4.4)
**Purpose:** Combine feedback with a feedforward path.

| Concept | One-line gloss |
|---|---|
| Feedforward | Add a control term computed from the reference (or a measured disturbance), not from the error. [Stated §4.4] |
| Model inversion | The feedforward provides the effort the plant model says is needed; feedback corrects for model error and disturbances. [Stated §4.4] |
| DC-gain inverse | Example 4.10 adds *G*⁻¹(0)·*r* to a DC motor loop to remove the steady-state tracking error to a step. [Stated §4.4] |

**Prerequisites:** Module A; Module B (steady-state error); Ch. 3 DC gain.

---

### Module E — Optional / web (§4.5, §4.6)
| Concept | One-line gloss |
|---|---|
| Digital control intro (▲, web W4.5) | Computers approximate continuous controllers with **difference equations**; Matlab `c2d` gives a discrete equivalent. Full treatment in Ch. 8. [Stated §4.5, Summary] |
| Sensitivity of time response (▲, web W4.6) | How the step response changes with parameters. [Stated §4.6] |

---

## 3. Dependency Map

```
Ch.3: transfer function, block diagrams, characteristic eq., FVT, stability
      │
      ▼
Module A ★: open loop vs unity feedback (R, W, V)
      loop gain GDc → S = 1/(1+GDc), T = GDc/(1+GDc), S + T = 1
      → stability · tracking · regulation (noise conflict) · sensitivity
      │
      ├──────────────────────┐
      ▼                      ▼
Module B ★: system type   Module D: feedforward (model inversion)
  polynomial inputs           (needs DC gain + Module B's idea of
  type = # integrators         steady-state error)
  Kp, Kv, Ka; robustness
  disturbance type
      │
      ▼
Module C ★: PID
  P → I (zero error, from Module B) → D (damping) → PID forms
  → reaction curve → Ziegler–Nichols (two methods)

Module E: digital (web) / time-response sensitivity (web)   (side topics)
```

**Must be understood first:** the unity-feedback loop and the definitions of *S* and *T*.

**Can be learned independently:**
- Feedforward (D) and PID (C) do not depend on each other.
- The two Ziegler–Nichols methods are independent of each other.
- Module E can be skipped entirely; Ch. 8 replaces §4.5.

**Depend on several earlier concepts:**
- "Integral control gives zero error to a constant disturbance" needs system type (B) **and** disturbance type, which depends on where *W* enters (A).
- Ziegler–Nichols' ultimate method needs PID (C) **and** the stability boundary from Ch. 3 (Routh / *j*ω-axis poles).
- The noise–disturbance conflict needs *S* **and** *T* **and** *S* + *T* = 1 (A).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Unity-feedback loop with *R*, *W*, *V*; loop gain; *S*, *T*, *S* + *T* = 1; characteristic equation 1 + *GD*꜀ = 0; the four effects of feedback; noise vs disturbance conflict; system type and *K*ₚ, *K*ᵥ, *K*ₐ; P, I, D effects; PID transfer function |
| **Supporting** | Open-loop analysis of the four objectives; sensitivity of *T* to *G*; disturbance type; robustness of type; derivative in the feedback path; Ziegler–Nichols tables; feedforward by DC-gain inversion |
| **Advanced / postpone** | Truxal's formula; digital control (web, see Ch. 8); time-response sensitivity (web); history (§4.7) |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- Ch. 3: block-diagram reduction, FVT, Routh, second-order specs.
- Limits: evaluating lim_{*s*→0} of rational functions.
- The idea of a pole "at the origin" = an integrator (1/*s*).

### Terms that need clarification
| Term | Issue |
|---|---|
| "Sensitivity" | Two roles for the same *S*: (1) it multiplies the reference and disturbance in the error; (2) it equals the relative sensitivity of *T* to *G*. Ch. 6 §6.7.7 studies \|*S*(*j*ω)\| across frequency. |
| *W* and *V* | In this book *W* = plant disturbance and *V* = sensor noise. FBS uses *v* for disturbance and *w* for noise — the opposite letters. |
| "Type" | Counted for the **loop** *GD*꜀ in unity feedback, not for the plant alone. For non-unity feedback, compute the error directly. |
| *T*ᵢ, *T*_D vs *k*ᵢ, *k*_D | Two parameterizations of the same PID: *k*ᵢ = *k*ₚ/*T*ᵢ, *k*_D = *k*ₚ*T*_D. |
| Ziegler–Nichols | Gives a *starting point* with low damping (quarter decay, ζ ≈ 0.21), not a finished design. [Stated §4.3.6] |

### Topics the chapter does not explain enough
- **Frequency-domain meaning** of *S* and *T* (which frequencies matter) waits for Ch. 6 §6.7.
- **Integrator windup** with actuator saturation is not covered here; it is in Ch. 9 §9.3.1. [Inferred cross-reference]
- **Practical derivative filtering**: a pure *k*_D *s* amplifies high-frequency noise. The book discusses noise for lead compensation in Ch. 5–6; treat a filtered derivative as standard practice. [Inferred]
- **Digital implementation** is web-only here; use Ch. 8.

### Note for Drillbotics [Inferred]
- **Module C is the first controller to build** for WOB and RPM loops. Integral action is what removes a steady WOB error caused by a constant disturbance (e.g., formation hardness).
- **Module A's noise conflict** matters for noisy sensors (e.g., hook-load or magnetometer readings): high loop gain rejects disturbances but passes sensor noise to the actuator.
- **Ziegler–Nichols' reaction-curve method** is a practical first tuning on the real rig, followed by refinement in simulation.
- **Feedforward (Module D)** fits when a set-point change is planned (e.g., a new target RPM).
