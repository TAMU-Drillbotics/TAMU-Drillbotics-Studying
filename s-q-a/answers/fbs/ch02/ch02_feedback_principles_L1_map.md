# Chapter 2: Feedback Principles — Layer 1 Knowledge Map

> Source: Åström & Murray, *Feedback Systems*, 2nd ed., Chapter 2, pp. 2-1 to 2-28.
> Layer: 1 (knowledge map). This file says what to learn and in what order, not the full explanations.
>
> **Tags:** **[Stated §x.y]** = the textbook says so directly. **[Inferred]** = my reading of the connection; check it against the text.

---

## 1. Domain Overview

### What does this chapter study?
It shows **four basic things negative feedback does**, using very simple models: [Stated, chapter intro]
1. **Disturbance attenuation**: reduce the effect of outside pushes.
2. **Reference tracking**: make the output follow a command.
3. **Robustness to uncertainty**: keep working when components vary.
4. **Shaping behavior**: change speed, damping, and linearity.

It also gives a first look at **positive feedback**.

### What problems does it address?
Chapter 1 *claimed* that feedback is useful. Chapter 2 *shows it with numbers*, using static (algebraic) models first and then simple first- and second-order differential equations.

### Why this chapter exists
The 2nd edition added it so that students can **design simple controllers early**, before the full theory. [Stated, Preface to 2nd ed.] The book says you may skip it and lose no continuity. **I recommend you do not skip it.** It is the most compact tour of the whole book's ideas. [Inferred]

### Major components
1. Static (no time) models of feedback (§2.1)
2. Linear dynamical models, the toolbox: ODE → characteristic polynomial → transfer function → block diagrams → stability test (§2.2)
3. Feedback for disturbance attenuation: P and PI control of a first-order process (§2.3)
4. Feedback for reference tracking, and two-degree-of-freedom controllers (§2.4)
5. Feedback for robustness: the feedback amplifier (§2.5)
6. Positive feedback: oscillators, memory, integral action (§2.6)

### After this chapter you should be able to:
- Show, with a static model, that negative feedback with gain *k* widens the linear range and cuts parameter sensitivity by a factor of *(k+1)*.
- Get a transfer function from a linear ODE, and combine blocks in series, in parallel, and in a feedback loop.
- Check stability of a low-order closed loop using the characteristic polynomial (Routh–Hurwitz for orders 2–3).
- Design a P or PI controller for a first-order process by choosing where the closed loop roots go.
- Explain why integral action removes steady-state error, and why unmodeled dynamics limit how fast you can make the loop.
- State what positive feedback does that negative feedback does not.

---

## 2. Knowledge Structure

### Module A — Static (algebraic) models of feedback (§2.1)
**Purpose:** Show the main benefits of feedback with *no* calculus, using one loop: gain *k* plus a saturating process.

| Concept | One-line gloss |
|---|---|
| Static model | Output depends only on the *current* input, not on history. [Stated §2.1] |
| Saturation, sat(x) | Linear for \|x\| < 1, clipped at ±1 outside. [Stated §2.1] |
| Linear range | The input range where the system acts linearly. [Stated §2.1] |
| Open loop gain *k* | Output/input ratio with no feedback. [Stated §2.1] |
| Closed loop gain *k/(k+1)* | Output/reference ratio with negative feedback. [Stated eq. 2.5] |
| Sensitivity (to a parameter) | Relative change of output ÷ relative change of parameter. [Stated §2.1] |
| Load disturbance *v* | A disturbance entering at the process input. [Stated §2.1] |
| Positive feedback (static) | Shrinks the linear range and raises the gain. At a critical gain it acts like a relay; above that it shows hysteresis. [Stated §2.1] |

**Key results [Stated §2.1]:** negative feedback (i) widens the linear range by *(k+1)*, (ii) reduces sensitivity to gain variation by *(k+1)*, (iii) reduces load disturbance effects, and (iv) **costs gain**: the closed loop gain is lower.

**Main limitation [Stated §2.1]:** static models **cannot show stability or instability**. That is why Module B is needed.

**Example:** superregenerative amplifier (Armstrong, 1914), positive feedback.

**Prerequisites:** Ch. 1 feedback vocabulary; algebra; the idea of a derivative (for the sensitivity *dy/dk*).

---

### Module B — Linear dynamical models: the core toolbox (§2.2) ★
**Purpose:** Introduce the tools for systems that change over time. These are the tools the whole book uses.

Learn these in this order. Each one builds on the one before it:

| # | Concept | One-line gloss |
|---|---|---|
| 1 | Linear ODE (eq. 2.10) | *y⁽ⁿ⁾ + a₁y⁽ⁿ⁻¹⁾ + … = b₀u⁽ᵐ⁾ + …*: the input/output model. [Stated §2.2] |
| 2 | Homogeneous equation and its solutions | The ODE with *u = 0*; describes the "free" motion. [Stated §2.2] |
| 3 | Characteristic polynomial *a(s)* | The polynomial whose roots *s_k* give the exponential modes *e^{s_k t}*. [Stated §2.2] |
| 4 | Exponential modes, time constant *T = 1/\|s_k\|* | Each real root gives a decaying or growing exponential; *T* says how fast. Complex roots give oscillations. [Stated §2.2] |
| 5 | Stability (of the ODE) | Stable if all homogeneous solutions → 0, i.e., all roots have negative real part. [Stated §2.2] |
| 6 | Routh–Hurwitz criterion | A test on polynomial *coefficients* to check if all roots are in the left half-plane, without solving for them. [Stated §2.2] |
| 7 | Exponential input *u = e^{st}* | A test input; a linear system answers with *G(s)e^{st}* (plus transients). [Stated §2.2] |
| 8 | Transfer function *G(s) = b(s)/a(s)* | The factor that scales the exponential input. [Stated §2.2] |
| 9 | Zero frequency (static) gain *G(0)* | Steady output ÷ constant input. [Stated §2.2] |
| 10 | Poles and zeros | Poles = roots of *a(s)* (the modes); zeros = roots of *b(s)* (exponentials that are blocked). [Stated §2.2] |
| 11 | Frequency response *G(iω)* | Steady-state response to sinusoids; can be measured experimentally. [Stated §2.2] |
| 12 | Block diagram algebra | Series → product; parallel → sum; feedback loop → *G/(1+GC)*-type formulas. [Stated §2.2] |
| 13 | Step response specs | Rise time, overshoot *M_p*, settling time *T_s*, steady-state value *y_ss*. [Stated §2.2] |
| 14 | Software (MATLAB `tf`, `series`, `parallel`, `feedback`) | Tools for doing the algebra by computer. [Stated §2.2] |

**Why these belong together:** They form one pipeline: **ODE → polynomial → modes / stability → transfer function → block diagram → closed loop response.**

**Note on approach [Stated, Preface 1st ed.]:** The book introduces transfer functions through *exponential inputs*, **not** Laplace transforms. Laplace appears only in an advanced section (§9.3). If you learned Laplace transforms before, both routes give the same *G(s)*.

**Prerequisites:** Module A (the closed loop idea); math: linear ODEs (homogeneous plus particular solution), complex numbers, polynomial roots.

**Formalized later:** Ch. 3 (models), Ch. 5 (stability), Ch. 6 (linear system response), Ch. 9 (transfer functions, poles and zeros, Bode plots).

---

### Module C — Feedback to attenuate disturbances (§2.3) ★
**Purpose:** The **regulation problem**: keep a variable near a constant setpoint despite disturbances. [Stated §2.3]

| Concept | One-line gloss |
|---|---|
| First-order process *dy/dt + ay = bu* | The simplest dynamic plant, used as the running example. [Stated §2.3] |
| Proportional control, *C(s) = k_p* | Speeds up the response (closed loop time constant *1/(a + bk_p)*) but leaves a **steady-state error** for constant disturbances. [Stated §2.3] |
| Steady-state error | The error that remains after transients die out. [Stated §2.3] |
| PI control | Adds integral action. A constant disturbance then leaves **zero** steady-state error. [Stated §2.3] |
| Closed loop characteristic polynomial | Its roots set closed loop speed and damping. With PI on a first-order plant, *k_p* and *k_i* can place both roots anywhere. [Stated §2.3] |
| Natural frequency ω_c, damping ratio ζ_c, damped frequency ω_d | Parameters of a 2nd-order polynomial *s² + 2ζω s + ω²*. They set oscillation speed and decay. [Stated §2.3] |
| Pole placement (first appearance) | Choose the controller gains to put the closed loop roots where you want them. [Stated §2.3. The general method is in Ch. 7] |
| Unmodeled dynamics | Neglected fast dynamics (sensors, actuators). They make "arbitrarily fast" designs unstable, so they **limit performance**. [Stated §2.3] |

**Why together:** One plant and one design problem, solved with increasing sophistication: P → PI → "how far can we push it?"

**Prerequisites:** Module B (characteristic polynomial, transfer function, stability); Ch. 1 §1.6 (P, I actions).

---

### Module D — Feedback to track reference signals (§2.4)
**Purpose:** The **servo problem**: make the output follow a changing command. [Stated §2.4]

| Concept | One-line gloss |
|---|---|
| Servo problem vs regulation problem | Following a command vs rejecting disturbances. [Stated §2.3–2.4] |
| *G_yr(s)*: reference-to-output transfer function | *G_yr(0) = 1* means constant references are tracked perfectly. [Stated §2.4] |
| Bandwidth | The frequency range over which the output tracks the reference well. [Stated §2.4] |
| Error feedback | The controller sees only *e = r − y*. [Stated §2.4] |
| Two degrees of freedom (2DOF) controller | Uses *r* and *y* separately (e.g., setpoint weighting), so the response to commands can be shaped independently of disturbance rejection. Expanded in §12.4. [Stated §2.4] |

**Prerequisites:** Modules B and C (the same PI loop is reused).

---

### Module E — Feedback to provide robustness (§2.5)
**Purpose:** Show how to build precise systems from imprecise parts: Black's feedback amplifier. [Stated §2.5]

| Concept | One-line gloss |
|---|---|
| Feedback amplifier | A high-gain, variable, nonlinear amplifier plus negative feedback gives a precise, linear closed loop gain. [Stated §2.5] |
| Reducing parameter variation and nonlinearity | The closed loop characteristic is set mostly by the *feedback element*, not by the amplifier. [Stated §2.5] |
| Nonlinear analysis by approximation | Useful insight comes from approximating the nonlinear loop. [Stated §2.5] |
| Closed loop time constant vs component dynamics | The amplifier's own dynamics must be fast compared to the desired closed loop time constant. [Stated §2.5] |

**Prerequisites:** Module A (static sensitivity result); Module B (dynamics).

**Relationship [Inferred]:** Module E is the dynamic version of Module A's static robustness result.

---

### Module F — Positive feedback (§2.6)
**Purpose:** Show the "complementary" uses of positive feedback. [Stated §2.6]

| Concept | One-line gloss |
|---|---|
| Exponential growth and instability | Positive feedback alone gives unbounded growth. [Stated §2.6] |
| Need for nonlinear analysis | Without nonlinearity (e.g., saturation) the growth never stops; linear analysis is not enough. [Stated §2.6] |
| Oscillator (Hewlett's oscillator) | Positive feedback plus amplitude limiting → a stable sustained oscillation. [Stated §2.6] |
| Integral action by positive feedback | Early controllers built "reset" (integral) action as positive feedback around a first-order lag. [Stated §2.6] |
| Positive feedback + saturation → memory | A bistable circuit that "remembers" a state. [Stated §2.6] |

**Prerequisites:** Module A (static positive feedback, hysteresis); Module B (exponential modes, stability).

---

## 3. Dependency Map

```
Ch.1 vocabulary (feedback, P/I/D, r, y, e, v)
        │
        ▼
Module A: Static models ── shows: linear range ↑, sensitivity ↓, gain ↓
        │                   (limit: no stability)
        ▼
Module B: Linear dynamical toolbox ★
   ODE → char. polynomial → modes/time const → STABILITY (Routh–Hurwitz)
     └→ exponential input → TRANSFER FUNCTION → G(0), poles/zeros, G(iω)
                                  └→ BLOCK DIAGRAM ALGEBRA → step-response specs
        │
        ├──────────────┬──────────────────┬────────────────────┐
        ▼              ▼                  ▼                    ▼
Module C:        Module D:           Module E:            Module F:
Disturbances     Tracking            Robustness           Positive feedback
P → PI →         G_yr, bandwidth,    feedback amplifier   oscillator, memory,
pole placement → 2DOF                (needs A + B)        integral via +FB
unmodeled dyn.   (needs C's PI loop)                      (needs A + B)
```

**Must be understood first:** Module B items 1–5 (ODE, characteristic polynomial, modes, stability), then 7–12 (transfer function, block diagrams).

**Can be learned independently (after B):** Modules E and F do not depend on C or D.

**Depend on several earlier concepts:**
- Module C needs B (polynomial roots, transfer functions) **and** Ch. 1's PI idea.
- Module D needs B **and** C (it reuses the PI loop from §2.3).
- 2DOF control (in D) needs error feedback (D) **and** the PI design (C).
- Unmodeled dynamics (in C) needs pole placement (C) **and** stability (B).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Negative feedback trades gain for linearity and robustness (Module A results); characteristic polynomial; stability = roots in the left half-plane; transfer function; poles; zero frequency gain *G(0)*; block diagram algebra for a feedback loop; P vs PI on a first-order plant; steady-state error; integral action removes it; regulation vs servo problem |
| **Supporting** | Routh–Hurwitz for 2nd and 3rd order; zeros; frequency response *G(iω)*; ω, ζ, ω_d parameters; step response specs (*M_p*, *T_s*); bandwidth; unmodeled dynamics as a performance limit; 2DOF controllers; feedback amplifier |
| **Advanced / postpone** | Static positive feedback, relay, and hysteresis; superregenerative amplifier; Hewlett's oscillator; integral action via positive feedback; memory circuits; nonlinear approximation details |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Linear ODEs with constant coefficients:** homogeneous and particular solutions, characteristic equation. *The book assumes this.* [Stated, Preface 1st ed.]
- **Complex numbers:** roots *σ ± iω*, the exponential *e^{(σ+iω)t}*, magnitude and phase.
- **Polynomials:** roots and factoring; the quadratic formula.
- **Basic calculus:** derivative, integral, and the chain rule (for sensitivity).

### Terms that need clarification
| Term | Issue |
|---|---|
| "Gain" | Means static gain *k* in §2.1, *G(0)* in §2.2, and loop gain later. Keep track of *which* gain. |
| "Stable" | §2.2 defines stability for a *linear ODE* only. Ch. 5 gives the general definition (equilibria, Lyapunov). |
| *s* | Here *s* is the exponent of a test signal *e^{st}*. In Laplace-based books *s* is the transform variable. The two meanings are consistent, but they are introduced differently. |
| Pole vs root of the characteristic polynomial | The same thing if there is no pole-zero cancellation. The book points out the distinction later (Ch. 9). |
| Regulation vs servo | The book's names for the disturbance-rejection and tracking problems. |

### Topics the chapter does not explain enough (by design)
- **Why** *e^{st}* in gives *G(s)e^{st}* out: derived here, but the full theory (transients, initial conditions) is in Ch. 6 and Ch. 9.
- **Routh–Hurwitz** is given only as a "hook": a short statement without proof. [Stated, Preface 2nd ed.]
- **How fast** you can make the loop before unmodeled dynamics bite: only shown by example. The general answer comes in Ch. 10 (stability margins) and Ch. 14 (fundamental limits).
- **Nonlinear analysis** of positive feedback systems is only sketched. Ch. 5 gives the tools (equilibria, limit cycles).
- The **frequency response** is introduced in one paragraph. The full Bode plot treatment is in §9.6.

### Note for Drillbotics [Inferred]
Module C (PI on a first-order plant, with a steady-state error check) is the template for a first WOB or RPM loop. Module C's *unmodeled dynamics* warning applies directly to motor and drillstring dynamics that a simple model leaves out.
