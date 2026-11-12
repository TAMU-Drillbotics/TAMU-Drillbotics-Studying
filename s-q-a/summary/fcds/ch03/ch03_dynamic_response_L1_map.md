# Chapter 3: Dynamic Response — Layer 1 Knowledge Map

> Source: Franklin, Powell & Emami-Naeini, *Feedback Control of Dynamic Systems*, 7th ed., Chapter 3, pp. 84–179, plus Appendix A (Laplace Transforms), pp. 804–818.
> Layer: 1 (knowledge map). What to learn and in what order, not full explanations.
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **how to find and interpret the time response of a linear system** from its model. The main tool is the **Laplace transform**, which turns differential equations into algebra. [Stated, Chapter Overview] It then links the **poles and zeros** of a transfer function to the shape of the step response, and defines **stability**.

### What problems does it address?
- How to solve linear ODEs quickly and gain insight into *why* the response looks the way it does. [Stated, Perspective]
- How to combine many subsystem models into one (block-diagram reduction).
- How to predict rise time, overshoot, and settling time **without** solving the ODE in full.
- How to decide whether a system is stable, and for which gains a feedback loop stays stable (Routh's test).

### Major components
1. Linear time-invariant (LTI) foundations: superposition, impulse response, convolution (§3.1.1)
2. Transfer function and frequency response (§3.1.2)
3. Laplace transform tools: definition, properties, partial fractions, Final Value Theorem (§3.1.3–3.1.7, Appendix A)
4. Poles and zeros (§3.1.8) and Matlab analysis (§3.1.9)
5. Block diagrams and their reduction (§3.2)
6. Pole locations → time response; time-domain specifications (§3.3, §3.4) ★
7. Effects of extra zeros and poles (§3.5)
8. Stability: definition, BIBO, internal stability, Routh's criterion (§3.6) ★
9. System identification and scaling (§3.7, §3.8 — web only)

The book says the three "domains" for studying response are **the s-plane, the frequency response, and the state space**, treated in depth in Ch. 5, 6, and 7. This chapter gives the tools needed before them. [Stated, Perspective]

### After this chapter you should be able to:
- Find the transfer function of a linear ODE and the output *Y*(*s*) = *G*(*s*)*U*(*s*).
- Invert a Laplace transform by partial fractions; use the Final Value Theorem correctly (only when it applies).
- Reduce a block diagram, including the feedback formula *G*/(1 + *GH*).
- Read ζ, ωₙ, σ, ω_d from a pole location and estimate *t*ᵣ, *M*ₚ, *t*ₛ.
- Predict qualitatively what an extra zero (LHP or RHP) or an extra pole does to a step response.
- Decide stability from pole locations, and find the range of gain *K* for stability using a Routh array.

---

## 2. Knowledge Structure

### Module A — LTI foundations (§3.1.1)
**Purpose:** Explain the two properties that make all the analysis possible. [Stated §3.1]

| Concept | One-line gloss |
|---|---|
| Superposition | Response to *a*₁*u*₁ + *a*₂*u*₂ is *a*₁*y*₁ + *a*₂*y*₂. Holds **if and only if** the system is linear. [Stated §3.1.1] |
| Time invariance | The system's parameters do not change with time. [Stated §3.1] |
| Unit impulse δ(*t*) | An idealized very short, very tall pulse with unit area. [Stated §3.1.1] |
| Impulse response *h*(*t*) | The output when the input is δ(*t*). [Stated §3.1.1] |
| Convolution | For an LTI system, *y*(*t*) = ∫ *h*(τ) *u*(*t* − τ) dτ. The impulse response fully describes the system. [Stated §3.1.1] |
| Exponential in → exponential out | From convolution, the response of an LTI system to *e*^{*st*} is *H*(*s*)*e*^{*st*}. This is why Laplace and Fourier transforms are so useful. [Stated §3.1] |

**Why together:** These properties define what "LTI" buys you.

**Prerequisites:** Ch. 2 (ODE models); integrals.

---

### Module B — Transfer function and frequency response (§3.1.2)
**Purpose:** Define the central object of classical control.

| Concept | One-line gloss |
|---|---|
| Transfer function *H*(*s*) | The Laplace transform of the impulse response; also the ratio *Y*(*s*)/*U*(*s*) with zero initial conditions. [Stated §3.1.2] |
| Frequency response | The steady response to a sinusoid. Evaluate *H*(*j*ω): the output sine is scaled by \|*H*(*j*ω)\| and shifted by ∠*H*(*j*ω). [Stated §3.1.2] |
| Systems that have transfer functions | Linear, time-invariant ones (Review Q 3.2). [Stated, Review Questions] |

**Why together:** Both come from "exponential in → exponential out" (Module A).

**Prerequisites:** Module A; complex numbers (magnitude, angle, *j*).

**Forward link [Stated, Perspective]:** frequency response is developed fully in Ch. 6.

---

### Module C — Laplace transform tools (§3.1.3–3.1.7, Appendix A)
**Purpose:** The calculation machinery.

| Concept | One-line gloss |
|---|---|
| ℒ₋ Laplace transform | *F*(*s*) = ∫₀₋^∞ *f*(*t*) *e*^{−*st*} d*t*. The lower limit 0⁻ includes impulses at *t* = 0. [Stated §3.1.3, Summary] |
| Key property | ℒ{ḟ} = *sF*(*s*) − *f*(0⁻): differentiation becomes multiplication by *s*. [Stated, Summary] |
| Tables and properties | Table A.1 (properties) and Table A.2 (pairs) in Appendix A. [Stated, Summary] |
| Inverse transform by partial fractions | Split *F*(*s*) into simple terms, look each up in the table; Matlab `residue` can do it. [Stated §3.1.5] |
| Final Value Theorem (FVT) | lim *y*(*t*) = lim_{*s*→0} *sY*(*s*), **only if all poles of *sY*(*s*) are in the LHP**. Main use: steady-state error. [Stated §3.1.6, Summary] |
| DC gain | The steady output for a unit step input: *G*(0). [Stated §3.1.6] |
| Initial Value Theorem | In Appendix A. [Stated, App. A.1.3] |
| Solving ODEs | Transform → algebra → partial fractions → inverse transform. [Stated §3.1.7] |

**Why together:** All are steps of the same "transform, solve, invert" workflow.

**Prerequisites:** Module B; algebra of rational functions; complex roots of polynomials.

---

### Module D — Poles and zeros (§3.1.8–3.1.9)
**Purpose:** Name the numbers that describe a transfer function.

| Concept | One-line gloss |
|---|---|
| Pole | A root of the denominator of *G*(*s*). Each pole gives a term like *e*^{*pt*} in the response. [Stated §3.1.8, §3.6.2] |
| Zero | A root of the numerator. Zeros change how much each pole term contributes. [Stated §3.1.8] |
| Characteristic equation | Denominator polynomial = 0. Its roots are the poles. [Stated §3.6.2] |
| Matlab tools | `tf`, `step`, `impulse`, `pole`, `zero`, `pzmap`, etc. [Stated §3.1.9] |

**Prerequisites:** Module C.

---

### Module E — Block diagrams (§3.2)
**Purpose:** Combine subsystem transfer functions into one.

| Concept | One-line gloss |
|---|---|
| Series, parallel connections | Multiply / add transfer functions. [Stated §3.2.1] |
| Negative feedback loop | Closed-loop gain = *G*₁/(1 + *G*₁*G*₂). "Negative feedback is usually required for system stability." [Stated §3.2.1] |
| Unity feedback system | The feedback path has gain 1. Any loop can be redrawn this way. [Stated §3.2.1] |
| Block-diagram reduction | Move blocks and summing points with standard rules (Fig. 3.10). Also with Matlab (`series`, `feedback`). [Stated §3.2.1–3.2.2] |
| Mason's rule / signal-flow graph | Alternative to reduction; web only (Appendix W3.2.3). [Stated §3.2.3] |

**Prerequisites:** Module B (transfer function); Ch. 1 (block diagram idea).

**Independent of Modules C–D for the algebra** (you can reduce diagrams without inverting transforms). [Inferred]

---

### Module F — Pole locations and time-domain specs (§3.3, §3.4) ★ the core of the chapter
**Purpose:** Read the response directly from the s-plane.

Learn these in order:

| # | Concept | One-line gloss |
|---|---|---|
| 1 | s-plane picture | Poles in the left half-plane (LHP) give decaying terms; right half-plane (RHP) growing; imaginary axis constant or oscillating (Fig. 3.16). [Stated §3.3] |
| 2 | First-order system and time constant τ | Pole at *s* = −σ gives *e*^{−σ*t*}; τ = 1/σ. [Stated §3.3] |
| 3 | Second-order system | *G*(*s*) = ωₙ²/(*s*² + 2ζωₙ*s* + ωₙ²). [Stated §3.3] |
| 4 | ζ, ωₙ, σ, ω_d | Damping ratio ζ, undamped natural frequency ωₙ (distance from origin), decay rate σ = ζωₙ (real part), damped frequency ω_d = ωₙ√(1 − ζ²) (imaginary part) (Fig. 3.18). [Stated §3.3] |
| 5 | Step-response specs | Rise time *t*ᵣ, overshoot *M*ₚ, peak time *t*ₚ, settling time *t*ₛ (Fig. 3.23). [Stated §3.4] |
| 6 | Approximate formulas | *t*ᵣ ≈ 1.8/ωₙ; *M*ₚ = *e*^{−πζ/√(1−ζ²)}; *t*ₛ ≈ 4.6/(ζωₙ) (1% settling). Valid for a second-order system with **no zeros**. [Stated, Summary] |
| 7 | Specs → s-plane region | Each spec becomes a boundary in the s-plane: ωₙ ≥ …, ζ ≥ …, σ ≥ …. [Stated §3.4] |

**Why together:** They translate between "where the poles are" and "what the step response looks like."

**Prerequisites:** Module D (poles); complex numbers in polar form.

**Forward link [Stated, Ch. 5 Perspective]:** the root locus (Ch. 5) is built on exactly this s-plane ↔ response relationship.

---

### Module G — Effects of extra zeros and poles (§3.5)
**Purpose:** Correct the second-order formulas when the system has more dynamics.

| Concept | One-line gloss |
|---|---|
| LHP zero | Increases overshoot, especially when it is near the real part of the complex poles. [Stated §3.5, Summary] |
| RHP zero (nonminimum phase) | The step response starts in the **wrong direction** and is more sluggish. [Stated §3.5, Summary] |
| Extra stable pole | Makes the response more sluggish; rise time increases significantly if the pole is within a factor of about 4 of the complex poles' real part. [Stated §3.5] |
| Dominant poles | The poles closest to the imaginary axis dominate the response when other poles/zeros are far away. [Inferred from §3.5 discussion] |

**Prerequisites:** Module F.

---

### Module H — Stability (§3.6) ★
**Purpose:** Define stability precisely and test it.

| Concept | One-line gloss |
|---|---|
| Stability of an LTI system | Stable if **all poles have negative real parts** (strictly in the LHP); unstable otherwise. [Stated §3.6] |
| BIBO stability | Every bounded input gives a bounded output; equivalent to ∫\|*h*(*t*)\|d*t* < ∞. Example: a capacitor (1/*s*) is not BIBO stable. [Stated §3.6.1] |
| Internal stability | All poles of the system (including any hidden by a pole–zero cancellation) are in the LHP. A cancelled RHP pole still appears "in some internal variable." [Stated §3.6.2] |
| Neutral stability | Non-repeated poles on the *j*ω axis: response neither grows nor decays (integrator, undamped oscillator). Repeated *j*ω poles → unstable. [Stated §3.6.2] |
| *j*ω axis = stability boundary | [Stated §3.6.2] |
| Routh's criterion | From the coefficients of the characteristic polynomial, build the Routh array; **stable iff all first-column elements are positive**. Used to find the range of a gain *K* for stability. [Stated §3.6.3, Summary] |
| Routh special cases | A zero in the first column or a zero row needs special handling (web Appendix W3.6.3). [Stated §3.6.3] |
| Kharitonov theorem | Tests stability when each coefficient is only known within a range, using four polynomials. Mentioned only. [Stated §3.6.3] |

**Why together:** All answer "does the response die out?"

**Prerequisites:** Module D (poles, characteristic equation); Module E (closed-loop transfer function, to apply it to feedback loops).

**Forward links [Stated §3.6]:** Nyquist's criterion (Ch. 6) and Lyapunov stability (Ch. 9) are more advanced stability tests.

---

### Module I — Models from data and scaling (§3.7, §3.8; web only)
**Purpose:** Named for completeness. System identification (build a model from experiments) and amplitude/time scaling are in web Appendices W3.7 and W3.8. [Stated §3.7–3.8]

---

## 3. Dependency Map

```
Module A: LTI foundations (superposition, impulse response, convolution)
      │
      ▼
Module B: Transfer function H(s); frequency response H(jω)
      │
      ├──────────────────────────────┐
      ▼                              ▼
Module C: Laplace tools          Module E: Block diagrams
  (ℒ, partial fractions,           (series, parallel, feedback G/(1+GH),
   FVT, DC gain)                    unity feedback, reduction)
      │                              │
      ▼                              │
Module D: Poles & zeros,             │
          characteristic equation    │
      │                              │
      ├──────────────┐               │
      ▼              ▼               │
Module F ★       Module H ★ ◄────────┘ (stability of closed loops needs the
pole location     stability, BIBO,       closed-loop transfer function)
→ ζ, ωn, σ        internal, Routh
→ tr, Mp, ts
      │
      ▼
Module G: extra zeros (LHP / RHP) and extra poles
```

**Must be understood first:** transfer function; poles; the left/right half-plane picture.

**Can be learned independently:**
- Block diagrams (E) and Laplace inversion (C) do not depend on each other.
- Routh's criterion (H) does not need the time-domain specs (F).
- Convolution (A) can be skimmed on first reading without blocking Modules C–H.

**Depend on several earlier concepts:**
- Routh-based gain range for a feedback loop needs block-diagram reduction (E) **and** the characteristic equation (D).
- Steady-state error via FVT needs the Laplace tools (C) **and** stability (H), because the FVT is only valid for stable *sY*(*s*).
- Effects of extra zeros/poles (G) need the second-order specs (F) **and** partial fractions (C).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Transfer function; Laplace transform and its derivative property; partial fractions; FVT and its condition; DC gain; poles and zeros; characteristic equation; negative-feedback formula; ζ, ωₙ, σ, ω_d; *t*ᵣ, *M*ₚ, *t*ₛ formulas; stability = all poles in LHP; Routh's criterion |
| **Supporting** | Superposition; impulse response; convolution; frequency response (first look); block-diagram reduction rules; LHP/RHP zero and extra-pole effects; BIBO vs internal stability; neutral stability; Matlab commands |
| **Advanced / postpone** | Routh special cases; Kharitonov theorem; Mason's rule; system identification and scaling (web); history (§3.9) |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Complex numbers:** rectangular and polar form, magnitude, angle, conjugates, *e*^{*j*θ}. The book's review is in web Appendix WA (not in this PDF).
- **Polynomials:** finding roots; factoring quadratics with complex roots.
- **Integration:** for the Laplace definition and convolution.
- **ODEs:** homogeneous solution as a sum of exponentials.

### Terms that need clarification
| Term | Issue |
|---|---|
| LHP / RHP | Left / right half of the complex s-plane (negative / positive real part). |
| "Stable" | The book's main definition is pole-based (LTI only). BIBO stability and internal stability are two refinements; they differ when a pole is cancelled by a zero. |
| Nonminimum phase | Here: has an RHP zero. Ch. 6 explains the name (extra phase lag for the same gain). |
| 0⁻ | "Just before *t* = 0." It lets the transform include an impulse applied at *t* = 0. |
| *M*ₚ, *t*ᵣ, *t*ₛ formulas | Approximations for a pure second-order system with no zeros. They are design starting points, not exact results. |

### Topics the chapter does not explain enough
- **Complex-variable background** is on the web only (Appendix WA).
- **System identification** (§3.7) and **scaling** (§3.8) are web only.
- **Frequency response** is only introduced; Ch. 6 does the real work.
- **State-space response** (matrix exponential) is not here; it is in web §7.14. FBS Ch. 6 covers it in print.

### Note for Drillbotics [Inferred]
- Modules F and H are the daily tools for tuning WOB/RPM loops: specs such as "settle within *X* s with less than *Y*% overshoot" turn into s-plane regions.
- An RHP zero (Module G) shows up when an actuator first pushes the output the "wrong way." Watch for it in step tests on the rig.
- Routh (Module H) gives a quick hand check of the maximum stable controller gain before simulating.
