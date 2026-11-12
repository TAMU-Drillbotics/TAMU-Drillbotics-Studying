# Chapter 9: Transfer Functions — Layer 1 Knowledge Map

> Source: Åström & Murray, *Feedback Systems*, 2nd ed., Chapter 9, pp. 9-1 to 9-42.
> Layer: 1 (knowledge map).
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies the **transfer function** *G(s)*: "a compact description of the input/output relation for a linear time-invariant system". It covers how to get it (analytically and experimentally), how to combine transfer functions with block diagrams, what poles, zeros, and gain mean, and how to draw it as a **Bode plot**. [Stated, chapter intro]

### Position in the book
This chapter **opens the second half of the book**: "classical control", the frequency domain, or input/output viewpoint. [Stated, Preface 1st ed.] Chapters 3–8 were mostly in state space. Chapters 9–14 mostly use transfer functions. This chapter is the **bridge** between the two views. [Inferred]

### What problems does it address?
- With many interconnected blocks, the differential equations become huge (Bode's 55th-order amplifier, chapter epigraph). Transfer functions turn interconnection into **algebra**. [Stated, epigraph and §9.4]
- How do we read system properties (steady-state gain, speed, oscillation, inverse response) directly from *G(s)*? (§9.5)
- How do we visualize behavior over a wide range of frequencies? (§9.6: Bode plot)
- How do we get a model from measurements? (§9.6: experimental frequency response)

### Major components
1. Frequency domain modeling: the idea (§9.1)
2. Determining the transfer function: exponential signals, ODEs, time delays, PDEs, realizations (§9.2) ★
3. Laplace transforms: the traditional route (§9.3, optional)
4. Block diagrams and transfer functions: block diagram algebra, closed loop transfer functions, algebraic loops (§9.4) ★
5. Zero frequency gain, poles, and zeros, including pole/zero cancellations (§9.5) ★
6. The Bode plot: sketching, right-half-plane poles and zeros, insights, experimental determination (§9.6) ★

### After this chapter you should be able to:
- Compute *G(s) = C(sI − A)⁻¹B + D* from a state space model, and *G(s) = b(s)/a(s)* from an ODE.
- Use block diagram algebra to get closed loop transfer functions (series, parallel, feedback).
- Read the zero frequency gain, poles, and zeros of *G(s)*, and say what each one implies about the time response.
- Explain why some pole/zero cancellations are dangerous.
- Sketch an asymptotic Bode plot for a rational transfer function, and interpret its gain and phase curves.
- Explain why right-half-plane zeros and time delays are special (they add phase lag without changing the gain).

---

## 2. Knowledge Structure

### Module A — Frequency domain modeling: the idea (§9.1)
**Purpose:** Motivate the input/output viewpoint. [Stated §9.1]

| Concept | One-line gloss |
|---|---|
| Input/output view of a control system | Process *P*, feedback controller *C*, feedforward *F*, connected by signals. [Stated §9.1, Fig. 9.1] |
| Why transfer functions | Inputs and outputs are what connect subsystems, so describing each block by its input/output map makes the overall behavior easier to understand. [Stated §9.1] |

**Prerequisites:** Ch. 3 (the input/output heritage); Ch. 6 (frequency response).

---

### Module B — Determining the transfer function (§9.2) ★
**Purpose:** Compute *G(s)* for many kinds of linear systems.

| # | Concept | One-line gloss |
|---|---|---|
| 1 | Exponential signal *e^{st}*, *s = σ + iω* | It includes constants, growing and decaying exponentials, and sinusoids as special cases. [Stated §9.2] |
| 2 | Transmission of exponential signals | Input *e^{st}* → output *G(s)e^{st}* + transient terms. [Stated §9.2] |
| 3 | Transfer function of a state space model, *G(s) = C(sI − A)⁻¹B + D* | [Stated §9.2] |
| 4 | Transfer function of an ODE, *G(s) = b(s)/a(s)* | Replace *d/dt* by *s*. [Stated §9.2] |
| 5 | Standard transfer functions (table) | Integrator *1/s*, differentiator *s*, first-order system, double integrator, damped oscillator, PID, time delay. [Stated §9.2 table] |
| 6 | Circuit elements as transfer functions | Impedance of R, L, C. [Stated §9.2, Ex. 9.3] |
| 7 | Time delay, *e^{−sτ}* | Not a rational function. It is "transcendental". [Stated §9.2] |
| 8 | PDE systems (heat propagation) | Also have transfer functions, e.g., *e^{−√s}*. [Stated §9.2] |
| 9 | State space realization | The inverse problem: transfer function → state space model (the realization problem). [Stated §9.2] |
| 10 | Proper / strictly proper | Degree of numerator ≤ (or <) degree of denominator. [Inferred: used in the realization discussion] |

**Examples:** damped oscillator (9.1), op amp circuit (9.2), vibration damper (9.4), time delay (9.5), heat propagation (9.6), pole/zero cancellation (9.7).

**Prerequisites:** Ch. 2 §2.2 (first look at the transfer function); Ch. 6 (convolution, frequency response); matrix inverse; complex numbers.

---

### Module C — Laplace transforms (§9.3) [optional bridge]
**Purpose:** Connect with the traditional derivation used by most other textbooks. [Stated §9.3; Preface: "only in an advanced section"]

| Concept | One-line gloss |
|---|---|
| Laplace transform *F(s) = ∫₀^∞ e^{−st} f(t) dt* | Turns differentiation into multiplication by *s*. [Stated §9.3] |
| Transfer function = transform of the impulse response | Ties back to the convolution in Ch. 6. [Stated §9.3, Ex. 9.9] |
| Initial conditions | The Laplace route carries *x(0)* explicitly (*sX − x(0)*). [Stated §9.3, Ex. 9.8] |

**Prerequisites:** Module B; improper integrals.

**Priority note:** Optional for this book. Useful if you also read Franklin–Powell (also in `materials/`), which uses Laplace throughout. [Inferred]

---

### Module D — Block diagrams and transfer functions (§9.4) ★
**Purpose:** Compute transfer functions of interconnected systems by algebra. [Stated §9.4]

| Concept | One-line gloss |
|---|---|
| Series connection | *G = G₂G₁*. [Stated §9.4] |
| Parallel connection | *G = G₁ + G₂*. [Stated §9.4] |
| Feedback connection | *G = G₁/(1 + G₁G₂)*. [Stated §9.4] |
| Superposition for several inputs | Total output = sum of the responses to each input. [Stated §9.4] |
| Control system transfer functions | With *P, C, F* and inputs *r, v (load disturbance), w (noise)*: transfer functions such as *G_yr = PCF/(1 + PC)*. [Stated §9.4] |
| "By inspection" rule | Common denominator *1 + PC*; numerator = product of the blocks along the forward path. [Stated §9.4] |
| Loop transfer function *L = PC* (first appearance) | The product of the blocks around the loop. [Stated §9.4; main use in Ch. 10] |
| Controller transfer functions *G_ur*, *G_uy* | An observer-based state feedback controller written as a transfer function. Implement it as one *n*th-order system, not two separate blocks. [Stated §9.4] |
| Algebraic loops | Loops with direct feedthrough everywhere. They complicate simulation. [Stated §9.4] |

**Example:** vehicle steering (9.10): state feedback + observer controller → transfer functions.

**Prerequisites:** Module B; Ch. 2 §2.2 (first block algebra); Ch. 8 (observer-based controller, for Ex. 9.10).

---

### Module E — Zero frequency gain, poles, and zeros (§9.5) ★
**Purpose:** Read key system properties directly from *G(s)*. [Stated §9.5]

| Concept | One-line gloss |
|---|---|
| Zero frequency gain *G(0)* | Steady-state output / step input amplitude. [Stated §9.5] |
| Poles (roots of *a(s)*) | Where *G(s)* is infinite. They are the modes *e^{pt}*. For a minimal state space model they equal the eigenvalues of *A*. [Stated §9.5] |
| Zeros (roots of *b(s)*) | Exponential inputs *e^{zt}* are blocked: the output is zero. [Stated §9.5] |
| Pole–zero diagram | Plot of the poles (×) and zeros (o) in the complex plane. [Stated §9.5] |
| Right-half-plane (RHP) poles / zeros | RHP poles mean instability. RHP zeros mean inverse response and limit performance (Ch. 14). [Stated §9.5–9.6] |
| Pole/zero cancellation | A common factor in *b(s)* and *a(s)*. If an **unstable** pole is canceled, the instability becomes hidden (unobservable or unreachable) but still there. [Stated §9.5] |
| Integral action viewed through zeros | A controller pole at *s = 0* forces a closed loop zero at *s = 0* in the error response, so constant signals are fully rejected. [Stated §9.5: "Effective use of zeros can be seen in integral control"] |

**Examples:** balance system (9.11), cruise control (9.12).

**Prerequisites:** Module B; Ch. 5 (eigenvalues ↔ stability); Ch. 7–8 (reachability and observability, to understand hidden modes).

---

### Module F — The Bode plot (§9.6) ★
**Purpose:** Visualize the frequency response over decades of frequency. [Stated §9.6]

| # | Concept | One-line gloss |
|---|---|---|
| 1 | Gain *M = \|G(iω)\|*, phase *φ = arg G(iω)* | The output is *M cos(ωt + φ)* for the input *cos ωt*. [Stated §9.6] |
| 2 | Bode plot | log *M* and *φ* vs log ω. [Stated §9.6] |
| 3 | Log scales turn products into sums | The Bode plot of *G₁G₂* = sum of the individual plots. [Stated §9.6] |
| 4 | Asymptotic sketching | Straight-line approximations with break (corner) frequencies at the poles and zeros. [Stated §9.6] |
| 5 | Low- and high-frequency asymptotes | Slopes set by the integrators and the relative degree. [Stated §9.6] |
| 6 | RHP poles and zeros on the Bode plot | Same gain as their LHP mirror images, but a different phase. [Stated §9.6] |
| 7 | Time delay on the Bode plot | Gain 1, phase −ωτ (it keeps falling). [Stated §9.6] |
| 8 | System insights | Read off low-frequency gain, bandwidth, resonances, and roll-off; sinusoidal decomposition of signals. [Stated §9.6] |
| 9 | Determining transfer functions experimentally | Inject sinusoids or noise, measure gain and phase (spectrum analysis), fit a model. [Stated §9.6] |

**Examples:** asymptotic sketch (9.13), RHP zero (9.14), spring–mass (9.15), transcriptional regulation (9.16), AFM (9.17), pupillary reflex (9.18).

**Prerequisites:** Module E (poles and zeros); Ch. 6 (frequency response); logarithms; complex numbers (magnitude and argument).

---

## 3. Dependency Map

```
Ch. 6: convolution, frequency response     Ch. 2 §2.2: G(s) via e^{st} (preview)
         └──────────────┬─────────────────────────┘
                        ▼
Module A: Why input/output (frequency-domain) modeling
                        │
                        ▼
Module B: TRANSFER FUNCTION
  e^{st} transmission → G(s) = C(sI−A)⁻¹B + D  and  G = b(s)/a(s)
  → table of standard TFs → time delay, PDEs → realization (inverse problem)
        │                     │
        │                     └──► Module C: Laplace (optional; parallel path)
        ▼
Module D: BLOCK DIAGRAM ALGEBRA → series/parallel/feedback → G_yr, G_yv, …, loop TF L = PC
        │                                                       │
        ▼                                                       │
Module E: G(0), POLES, ZEROS → RHP poles/zeros → pole/zero cancellation (needs Ch. 7–8)
        │                                                       │
        ▼                                                       ▼
Module F: BODE PLOT (gain/phase, asymptotes, RHP effects, delays, experiments)  → Ch. 10
```

**Must be understood first:** exponential signal → *G(s)* (both formulas) → poles and zeros.

**Can be learned independently:**
- Module C (Laplace) is an alternative path. Nothing later *requires* it. [Stated, Preface 1st ed.]
- Module D (block algebra) and Module E (poles and zeros) both need only Module B. They can be learned in either order.
- Time delays and PDEs (B7–B8) can be postponed, except that time delays matter again in Ch. 10–14.

**Depend on several earlier concepts:**
- Pole/zero cancellation danger (E) needs poles and zeros (E), stability (Ch. 5), **and** reachability/observability (Ch. 7–8): a canceled mode is unreachable or unobservable.
- The Bode plot (F) needs the frequency response (Ch. 6), poles and zeros (E), **and** logarithms/complex numbers.
- Controller transfer functions *G_ur, G_uy* (D) need the observer-based controller (Ch. 8) **and** block algebra (D).

**Forward links [Stated, Preface]:** the loop transfer function and the Bode plot → Ch. 10 (Nyquist, stability margins); poles and zeros → Ch. 12 (loop shaping), Ch. 14 (RHP limits).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | *G(s)* from state space and from an ODE; standard transfer functions (integrator, 1st order, 2nd order, PID, delay); series/parallel/feedback algebra; closed loop transfer functions with *P, C, F*; zero frequency gain; poles = modes, zeros = blocked signals; RHP pole = unstable; gain and phase; reading a Bode plot |
| **Supporting** | Exponential signal derivation; time delay *e^{−sτ}*; asymptotic Bode sketching; RHP zeros and delays add phase lag; pole/zero cancellation risk; experimental frequency response; loop transfer function (first look) |
| **Advanced / postpone** | Laplace transform section; PDE transfer functions (heat equation); the realization problem; algebraic loops; controller transfer functions from an observer; biological examples |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Complex numbers in polar form:** magnitude, argument, *e^{iω t}*, multiplying complex numbers (gains multiply, phases add).
- **Logarithms and decibels:** dB = 20·log₁₀ \|G\|. The book often plots log *M* directly.
- **Polynomial algebra:** roots, factoring, partial fractions (for Laplace).
- **Matrix inverse** and *(sI − A)⁻¹*.

### Terms that need clarification
| Term | Issue |
|---|---|
| *s* | Here *s* is the complex frequency of the exponential signal. In Laplace texts it is the transform variable. The resulting transfer functions are the same. |
| Pole vs eigenvalue | Equal for a minimal realization (no cancellations). Otherwise some eigenvalues are hidden. |
| Gain | Here *\|G(iω)\|*, which depends on frequency. Distinguish it from the zero frequency gain *G(0)* and the static gain *k* of Ch. 2. |
| Phase lag / lead | Negative phase = output lags input. |
| Minimum phase / nonminimum phase | Introduced through RHP zeros here; formally defined in §10.4. |
| Proper | Numerator degree ≤ denominator degree. Physical systems are (almost always) proper. |
| *P, C, F, v, w* | Process, feedback controller, feedforward, load disturbance, measurement noise (2nd edition notation). |

### Topics the chapter does not explain enough
- **The transient part** of the response to *e^{st}* is set aside. The transfer function describes only the forced response. [Stated §9.2: two components]
- **Detailed Bode sketching rules**: the book de-emphasizes hand-drawing rules. [Stated, Preface 1st ed.: "Topics that we have de-emphasized include … detailed rules for generating Bode and Nyquist plots by hand"]
- **Laplace transform tables and inverse transforms**: minimal. Use another text if your course relies on them.
- **System identification** beyond spectrum analysis: only examples.
- **MIMO transfer function matrices** are not developed.

### Note for Drillbotics [Inferred]
- **Measuring the frequency response** of the toy drill or lab rig motor (Module F, item 9) is a practical way to validate the Simscape model.
- The **time delay** (B7) matters if sensor data reaches the controller with latency (e.g., downhole-to-surface telemetry, or a slow comms link). Delays limit how aggressive the loop can be (Ch. 10, 14).
