# Chapter 8: Digital Control — Layer 1 Knowledge Map

> Source: Franklin, Powell & Emami-Naeini, *Feedback Control of Dynamic Systems*, 7th ed., Chapter 8, pp. 590–636.
> Layer: 1 (knowledge map). What to learn and in what order, not full explanations.
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **how to implement a controller on a digital computer**. Most controllers today run on microprocessors, not analog electronics. [Stated, Perspective] A computer works on **samples** of the sensor signal and computes the control with **difference equations** (algebraic recursions), not differential equations.

### What problems does it address? [Stated, Perspective and §8.1]
- How to turn a continuous controller *D*꜀(*s*) into code (difference equations).
- Sampling introduces an average **delay of half a sample period**, which reduces stability margins.
- Sampling causes **aliasing**: high-frequency signals look like low-frequency ones.
- How fast to sample.
- How to analyze a sampled system exactly (the z-transform), and how to design directly in discrete time.

### Major components
1. Digitization: the structure of a digital control loop (§8.1)
2. Discrete analysis: the z-transform and the s ↔ z relationship (§8.2) ★
3. Design by discrete equivalents: Tustin, ZOH, matched pole-zero (§8.3) ★
4. Hardware: A/D, D/A, anti-alias prefilters, the computer (§8.4)
5. Sample-rate selection (§8.5) ★
6. Discrete (direct digital) design (§8.6 ▲, §8.7 web)

### After this chapter you should be able to:
- Draw a digital control loop with A/D, computer, D/A with zero-order hold, plant, and sensor.
- Write and invert simple z-transforms; find a discrete transfer function from a difference equation.
- Map poles between planes with *z* = *e*^{*sT*}, and judge stability with the unit circle.
- Convert *D*꜀(*s*) to *D*_d(*z*) with Tustin's method or the matched pole-zero method (and `c2d`).
- Choose a sample rate (≥ 20× bandwidth for discrete-equivalent design) and an anti-alias prefilter.

---

## 2. Knowledge Structure

### Module A — Digitization (§8.1)
**Purpose:** Describe what changes when a computer is in the loop.

| Concept | One-line gloss |
|---|---|
| Digital control loop | Sensor → **A/D** (sample + convert) → computer (difference equations) → **D/A** → plant. [Stated §8.1, Fig. 8.1] |
| Sample period *T*, sample rate 1/*T* | The book assumes a fixed sample period. [Stated §8.1] |
| Difference equation | A recursive algebraic equation, e.g., *u*(*k*) = *u*(*k* − 1) + …, that approximates the continuous controller. [Stated §8.1] |
| Zero-order hold (ZOH) | Holds the D/A output constant over each sample period. [Stated §8.1] |
| Average delay *T*/2 | The held signal lags the ideal continuous signal by *T*/2 on average; this tends to destabilize. [Stated §8.1, Perspective] |
| Simulink evaluation | Simulate the discrete controller with the continuous plant to see the degradation. [Stated, Chapter Overview] |

**Prerequisites:** Ch. 4–7 (a continuous controller *D*꜀(*s*) to implement); Ch. 6 (delay costs phase).

---

### Module B — Discrete analysis: the z-transform (§8.2) ★
**Purpose:** The discrete-time counterpart of the Laplace transform.

| Concept | One-line gloss |
|---|---|
| z-transform | *F*(*z*) = Σ *f*(*k*)*z*⁻ᵏ. [Stated §8.2.1, Summary] |
| Key property | 𝒵{*f*(*k* − 1)} = *z*⁻¹*F*(*z*): *z*⁻¹ is a one-sample delay. [Stated, Summary] |
| Discrete transfer function | From a difference equation, using the key property. [Stated, Summary] |
| Inversion | Tables (Table 8.1), partial fractions, or long division; Matlab. [Stated §8.2.2] |
| s ↔ z mapping | *z* = *e*^{*sT*}. [Stated §8.2.3, Summary] |
| Unit circle = stability boundary | Inside \|*z*\| = 1 is stable, outside unstable (Fig. 8.5 shows response vs *z* location). [Stated §8.2.3] |
| Nyquist frequency ω_s/2 | Half the sample frequency. Signals above it are **aliased** (folded) to lower frequencies. [Stated §8.2.3] |
| Discrete Final Value Theorem | lim *x*(*k*) = lim_{*z*→1} (1 − *z*⁻¹)*X*(*z*), if all poles of (1 − *z*⁻¹)*X*(*z*) are inside the unit circle. [Stated §8.2.4, Summary] |

**Why together:** The full analysis toolkit for sampled systems; "Analysis using z-transforms closely parallels that using Laplace transforms." [Stated, Summary]

**Prerequisites:** Ch. 3 (Laplace transform, partial fractions, FVT, s-plane); geometric series.

---

### Module C — Design using discrete equivalents (§8.3) ★
**Purpose:** The simplest route: design *D*꜀(*s*) as before, then approximate it. [Stated, Summary]

| Concept | One-line gloss |
|---|---|
| Discrete-equivalent design | (a) Find *D*꜀(*s*) with Ch. 1–7 methods; (b) approximate it with a difference equation. [Stated, Summary] |
| Tustin's method (bilinear) | Substitute *s* = (2/*T*)(*z* − 1)/(*z* + 1). [Stated §8.3.1, Summary] |
| ZOH method | Assume the controller's input is held constant over each sample. [Stated §8.3.2] |
| Matched pole-zero (MPZ) | Map each pole and zero by *z* = *e*^{*sT*}, add (*z* + 1) factors as needed, match the low-frequency gain. [Stated §8.3.3, Summary] |
| Modified MPZ (MMPZ) | A variant that avoids needing the current input sample (allows computation time). [Stated §8.3.4] |
| Comparison and limits | All work when sampling is fast; they degrade as sampling slows. Recommended minimum: **20× the bandwidth**. [Stated §8.3.5–8.3.6, Summary] |
| Matlab `c2d` | Computes discrete equivalents. [Stated, Ch. 4 Summary] |

**Prerequisites:** Module B (*z* = *e*^{*sT*}); a continuous design *D*꜀(*s*).

---

### Module D — Hardware characteristics (§8.4)
| Concept | One-line gloss |
|---|---|
| A/D converter | Converts the sampled analog sensor signal into a digital word (a binary number). [Stated §8.4.1] |
| D/A converter | Converts the computed number back to a voltage (usually with a ZOH). [Stated §8.4.2] |
| Anti-alias prefilter | An **analog** low-pass filter before the sampler, so no significant content remains above half the sample rate. [Stated §8.4.3, Summary] |
| The computer | Computation time and word length affect implementation. [Stated §8.4.4] |

**Prerequisites:** Module A; Module B (Nyquist frequency, aliasing).

---

### Module E — Sample-rate selection (§8.5) ★
**Purpose:** Choose *T*.

| Concept | One-line gloss |
|---|---|
| Absolute minimum | Sampling at twice the bandwidth can in theory keep a discrete design stable. [Stated, Summary] |
| Practical rates | For good transients and disturbance rejection, sample at **≥ 10× the closed-loop bandwidth** (discrete design) and **≥ 20×** for discrete-equivalent design. [Stated, Summary] |
| Tracking effectiveness, disturbance rejection | Two criteria that push the rate up. [Stated §8.5.1–8.5.2] |
| Effect of the prefilter | The prefilter adds phase lag that must be accounted for. [Stated §8.5.3] |
| Asynchronous sampling | Multiple loops at different rates. [Stated §8.5.4] |
| Vibrational modes | Sometimes sample more than twice as fast as a troublesome vibration mode. [Stated, Summary] |

**Prerequisites:** Modules A–D; Ch. 6 (bandwidth).

---

### Module F — Discrete design (§8.6 ▲, §8.7 web)
**Purpose:** An exact method that avoids the approximations of discrete equivalents. [Stated, Summary]

| Concept | One-line gloss |
|---|---|
| Discrete plant model with ZOH | *G*(*z*) = (1 − *z*⁻¹)𝒵{*G*(*s*)/*s*}. [Stated, Summary] |
| Direct digital design | Design *D*_d(*z*) on *G*(*z*) with z-plane root locus / frequency response. Needs *T* chosen first. [Stated, Summary] |
| Practical approach | Start with discrete equivalents, then "tune up" with discrete design. [Stated, Summary] |
| Discrete state-space design | Web only (§8.7). [Stated §8.7] |

**Prerequisites:** Modules B, C, E; Ch. 5–6 methods in the z-plane.

---

## 3. Dependency Map

```
Ch.3 Laplace / s-plane      Ch.4–7: a continuous controller Dc(s)      Ch.6: bandwidth, phase lag
          │                              │                                  │
          ▼                              ▼                                  │
Module B ★: z-transform,         Module A: digital loop, A/D, D/A, ZOH,     │
  z⁻¹ = delay, discrete TF,      T/2 delay, difference equations            │
  z = e^{sT}, unit circle,                │                                 │
  aliasing, discrete FVT                  │                                 │
          │                               │                                 │
          └───────────────┬───────────────┘                                 │
                          ▼                                                 │
Module C ★: discrete equivalents (Tustin, ZOH, MPZ, MMPZ), c2d              │
                          │                                                 │
          ┌───────────────┴───────────────┐                                 │
          ▼                               ▼                                 │
Module D: hardware, anti-alias     Module E ★: sample-rate selection ◄──────┘
          prefilter ──────────────►  (2× theory; 10×–20× practice)
                          │
                          ▼
Module F (▲): discrete design: G(z) with ZOH, direct digital design
```

**Must be understood first:** what sampling and the ZOH do; *z*⁻¹ as a one-sample delay; *z* = *e*^{*sT*}.

**Can be learned independently:**
- Discrete equivalents (C) can be used in practice (via `c2d`) before mastering z-transform inversion. [Inferred]
- Hardware (D) is mostly descriptive and independent of Module C.

**Depend on several earlier concepts:**
- Sample-rate selection (E) needs the *T*/2 delay (A) **and** aliasing (B) **and** closed-loop bandwidth (Ch. 6).
- Prefilter design (D) needs aliasing (B) **and** the noise spectrum of the sensor.
- Discrete design (F) needs the z-transform (B) **and** a chosen sample rate (E) **and** the Ch. 5/6 design methods.

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Digital loop structure; sample period; ZOH; difference equation; *T*/2 delay; *z*⁻¹ = one-sample delay; *z* = *e*^{*sT*}; unit-circle stability; Nyquist frequency and aliasing; Tustin's method; `c2d`; sample at ≥ 20× bandwidth for discrete equivalents; anti-alias prefilter |
| **Supporting** | z-transform definition and inversion; discrete FVT; MPZ and ZOH methods; comparison of methods; A/D and D/A details; sample-rate criteria |
| **Advanced / postpone** | MMPZ; asynchronous sampling; discrete plant model *G*(*z*) with ZOH; direct digital design; discrete state-space (web); history (§8.8) |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- Ch. 3 Laplace transform and s-plane intuition (the z-plane is its mapped image).
- Geometric series Σ *aᵏ* (for simple z-transforms).
- Ch. 6 bandwidth and phase margin (to judge the effect of delay).

### Terms that need clarification
| Term | Issue |
|---|---|
| Nyquist frequency | Half the sample rate. Different from the Nyquist *plot* / *criterion* of Ch. 6 (same person, different idea). |
| Nyquist rate | Twice the highest signal frequency — the theoretical minimum sample rate (Review Q 8.1). |
| "Discrete equivalent" vs "discrete design" | Equivalent = approximate a continuous design; discrete design = design directly on a discrete plant model. |
| *T* | Sample period here; elsewhere in the book *T* is the complementary sensitivity. Context decides. |
| Bandwidth in "20× bandwidth" | The **closed-loop** bandwidth (ω_BW from Ch. 6), in the same units as the sample rate (rad/s vs Hz — be careful). |

### Topics the chapter does not explain enough
- The book calls its treatment "a limited version of a complex subject" and points to *Digital Control of Dynamic Systems* (Franklin, Powell & Workman). [Stated, Perspective]
- **Quantization** (finite word length) effects are not analyzed; §8.4 only describes converters and digital words. [Inferred from the text]
- **Discrete state-space design** is web only (§8.7).
- **Implementation details** such as integrator antiwindup in code, timing jitter, and variable sample periods are barely covered (antiwindup is in Ch. 9). [Inferred]

### Note for Drillbotics [Inferred]
- **This chapter is directly about how the team's controller will run**: the same controller code across sim, toy drill, and lab rig is a discrete-time controller at a fixed rate.
- Pick the loop rate from the closed-loop bandwidth (Module E) and make sure sensor filtering avoids aliasing (Module D) — relevant to the noisy magnetometer readings mentioned in the plan.
- In Simulink, use discrete blocks (or `c2d`/Tustin) so the simulated controller matches the deployed one.
- Account for the *T*/2 hold delay plus any communication delay when checking phase margin.
