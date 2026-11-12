# Chapter 10: Frequency Domain Analysis — Layer 1 Knowledge Map

> Source: Åström & Murray, *Feedback Systems*, 2nd ed., Chapter 10, pp. 10-1 to 10-28.
> Layer: 1 (knowledge map).
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **how to decide whether a closed loop is stable, and how robustly, by looking only at the *open loop* transfer function**. The method traces how sinusoids travel around the loop. The central result is the **Nyquist stability criterion**. [Stated, chapter intro]

### Why is this needed?
- In Ch. 5–8 stability came from closed loop eigenvalues. That requires a model of the whole closed loop, and it gives only a yes/no answer.
- The Nyquist approach (i) works from *open loop* data, which can be **measured** experimentally (Ch. 9 §9.6), (ii) handles **time delays** (not rational) naturally, and (iii) gives **margins**: a measure of *how far* the loop is from instability. [Stated §10.1–10.3; the comparison with eigenvalue methods is Inferred]
- Nyquist's epigraph: it was "puzzling" that a loop with gain far above 1 could be stable. This chapter explains why. [Stated, epigraph]

### Major components
1. The loop transfer function and loop analysis (§10.1) ★
2. The Nyquist criterion: simplified and general, conditional stability (§10.2) ★
3. Stability margins: gain, phase, stability margin (§10.3) ★
4. Bode's relations and minimum phase systems (§10.4)
5. Generalized gain and phase: passivity, small gain, circle criterion, describing functions (§10.5) [advanced]

### After this chapter you should be able to:
- Form the loop transfer function *L(s) = P(s)C(s)* and explain the critical point −1.
- Draw or read a Nyquist plot and apply the simplified Nyquist criterion (no encirclement of −1 when *L* has no RHP poles).
- Apply the general criterion (counting encirclements against the open loop RHP poles).
- Read gain margin, phase margin, and stability margin from Nyquist and Bode plots.
- Explain *minimum phase* and why time delays and RHP zeros add phase lag without changing the gain.
- (Advanced) Explain the small gain theorem and the describing function idea at a high level.

---

## 2. Knowledge Structure

### Module A — The loop transfer function (§10.1) ★
**Purpose:** Introduce *loop analysis*: reason about the closed loop through the open loop. [Stated §10.1]

| Concept | One-line gloss |
|---|---|
| Loop transfer function *L(s) = P(s)C(s)* | The transfer function obtained by "breaking the loop" and going once around it. [Stated §10.1] |
| Breaking the loop | Cut the loop at a point, inject a signal, and see what comes back. [Stated §10.1] |
| Critical point −1 | If *L(iω₀) = −1*, a sinusoid at ω₀ returns to the cut point exactly as it was sent, so it can sustain itself as an oscillation. [Stated §10.1] |
| Gain > 1 at phase −180° | Intuitively the signal grows; Nyquist showed this intuition is not the whole story. [Stated §10.1] |
| Closed loop poles = zeros of *1 + L(s)* | The link between the open loop *L* and closed loop stability. [Stated §10.2 proof] |

**Example:** DC motor with P control and a delay (10.1).

**Prerequisites:** Ch. 9 (transfer functions, block diagram algebra, the *L = PC* first mention; frequency response); Ch. 5 (stability).

---

### Module B — The Nyquist criterion (§10.2) ★
**Purpose:** A graphical stability test based on *L(iω)*. [Stated §10.2]

| # | Concept | One-line gloss |
|---|---|---|
| 1 | Nyquist contour Γ | A closed path that encloses the right half-plane: up the imaginary axis, around a large semicircle, with small detours around poles on the axis. [Stated §10.2] |
| 2 | Nyquist plot | The image of Γ under *L(s)*, essentially *L(iω)* for −∞ < ω < ∞. [Stated §10.2] |
| 3 | **Theorem 10.1** (simplified Nyquist) | If *L* has no poles in the closed RHP (except possibly at 0), the closed loop is stable ⇔ the Nyquist plot does **not encircle** −1. [Stated §10.2] |
| 4 | Winding number / encirclement | The net number of times the plot goes around −1. [Stated §10.2] |
| 5 | **Theorem 10.2** (principle of variation of the argument) | A complex-analysis result: winding number = zeros − poles inside the contour. [Stated §10.2] |
| 6 | **Theorem 10.3** (general Nyquist) | Closed loop RHP poles = encirclements of −1 + open loop RHP poles. For stability, the plot must encircle −1 **counterclockwise** once for each open loop RHP pole. [Stated §10.2] |
| 7 | Conditional stability | Some systems are stable only for a *range* of gain: both increasing and decreasing the gain can destabilize them. [Stated §10.2] |

**Examples:** third-order systems (10.2, 10.3), congestion control (10.4), stabilized inverted pendulum (10.5), conditional stability (10.6).

**Prerequisites:** Module A; complex numbers; for Theorems 10.2–10.3, complex variables (analytic functions). The book marks those parts as requiring that background. [Stated, Preface 1st ed.: "in some of the advanced sections, more detailed concepts in complex variables"]

---

### Module C — Stability margins (§10.3) ★
**Purpose:** Measure *how far* from instability a stable loop is. That is, how robust it is. [Stated §10.3]

| Concept | One-line gloss |
|---|---|
| Stability margin *s_m* | The shortest distance from the Nyquist curve to −1. [Stated §10.3] |
| Gain margin *g_m* | How much the loop gain can be multiplied before instability (measured at the phase crossover). [Stated §10.3] |
| Phase margin φ_m | How much extra phase lag can be tolerated before instability (measured at the gain crossover). [Stated §10.3] |
| Gain crossover frequency ω_gc | Where *\|L(iω)\| = 1*. [Stated §10.3] |
| Phase crossover frequency ω_pc | Where *arg L(iω) = −180°*. [Stated §10.3] |
| Reading margins from Bode plots | Margins appear directly on the Bode gain and phase curves. [Stated §10.3] |
| Delay margin | The extra time delay the loop can tolerate, related to φ_m / ω_gc. [Stated §10.3] |
| Good *g_m* and φ_m are not enough | Ex. 10.8: the curve can pass close to −1 between the crossovers, so check *s_m* too. [Stated §10.3] |
| Typical requirements | Ranges for reasonable margins (e.g., φ_m roughly 30°–60°). [Stated §10.3: typical values given] |

**Examples:** third-order system (10.7), good margins with poor *s_m* (10.8), AFM nanopositioning (10.9).

**Prerequisites:** Module B; Ch. 9 Bode plot.

---

### Module D — Bode's relations and minimum phase systems (§10.4)
**Purpose:** Show that for many systems the phase is *determined by* the gain curve. [Stated §10.4]

| Concept | One-line gloss |
|---|---|
| Bode's gain–phase relation | For minimum phase systems, the phase at ω is roughly (π/2) × the local slope of log\|G\| vs log ω. [Stated §10.4] |
| Minimum phase system | No RHP poles or zeros and no time delay. It has the least phase lag possible for its gain curve. [Stated §10.4] |
| Nonminimum phase system | Has RHP zeros and/or time delays, so it has extra phase lag that the gain curve does not show. [Stated §10.4] |
| Consequence for design | A steep gain roll-off at crossover brings a large phase lag, so a small phase margin. Nonminimum phase elements make this worse. [Inferred from §10.4; developed in Ch. 12, 14] |

**Examples:** *G = sⁿ* (10.10), vehicle steering forward vs reverse (10.11).

**Prerequisites:** Ch. 9 Bode plot, RHP zeros, time delay; Module C (phase margin).

---

### Module E — Generalized notions of gain and phase (§10.5) [advanced]
**Purpose:** Extend gain, phase, and Nyquist-style reasoning to nonlinear or more general systems. [Stated §10.5]

| Concept | One-line gloss |
|---|---|
| System gain | The largest ratio of output size to input size (signal norms). [Stated §10.5] |
| Passivity | A generalization of "phase within ±90°". It is energy-based. [Stated §10.5] |
| **Theorem 10.4** (small gain theorem) | If the product of the gains around a loop is < 1, the loop is stable. [Stated §10.5] |
| **Theorem 10.5** (circle criterion) | Stability of a linear system with a sector-bounded static nonlinearity, via a disk condition on the Nyquist plot. [Stated §10.5] |
| Describing function | An approximate "gain" of a nonlinearity for a sinusoidal input. It is used to **predict limit cycles** (e.g., relay with hysteresis). [Stated §10.5] |

**Example:** relay with hysteresis (10.12).

**Prerequisites:** Modules B and C; Ch. 5 (limit cycles); signal norms (new concept).

---

## 3. Dependency Map

```
Ch. 9: transfer functions, block diagrams, Bode plot, RHP zeros, delays
Ch. 5: stability, (limit cycles)
        │
        ▼
Module A: LOOP TRANSFER FUNCTION L = PC → critical point −1 → closed-loop poles = zeros of 1+L
        │
        ▼
Module B: NYQUIST PLOT → simplified criterion (Thm 10.1)
        │       └──► [complex variables] → argument principle (Thm 10.2) → general criterion (Thm 10.3)
        │                                                              → conditional stability
        ▼
Module C: STABILITY MARGINS  s_m, g_m, φ_m, crossovers, delay margin (on Nyquist & Bode)
        │
        ├──► Module D: Bode's gain–phase relation → minimum vs nonminimum phase
        │       (needs C + Ch. 9 RHP zeros/delays)
        │
        └──► Module E (advanced): system gain → small gain thm; passivity; circle criterion;
                describing functions (needs Ch. 5 limit cycles)
```

**Must be understood first:** *L = PC* → −1 as the critical point → simplified Nyquist → gain and phase margins.

**Can be learned independently:**
- The general Nyquist criterion (B5–B6) can be postponed. Most practical loops have stable *L*, so Theorem 10.1 is enough. [Inferred]
- Module D and Module E are independent of each other.
- Within E, describing functions are separate from the small gain / circle criterion results.

**Depend on several earlier concepts:**
- Margins (C) need the Nyquist plot (B) **and** the Bode plot (Ch. 9).
- The delay margin needs the phase margin (C) **and** the delay phase *−ωτ* (Ch. 9).
- Nonminimum phase (D) needs RHP zeros (Ch. 9), the Bode gain–phase relation (D), **and** the phase margin (C).
- Describing functions (E) need limit cycles (Ch. 5) **and** the Nyquist idea (B).

**Forward links [Stated, Preface 1st ed. / inferred from cross-references]:** margins → Ch. 11 (PID tuning targets), Ch. 12 (loop shaping specs), Ch. 13 (robust stability: *s_m* = 1/*M_s*); minimum phase → Ch. 14 (fundamental limits).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Loop transfer function *L = PC*; critical point −1; closed loop poles = zeros of *1 + L*; Nyquist plot; simplified Nyquist criterion (Thm 10.1); gain margin, phase margin, stability margin; gain and phase crossover frequencies; reading margins on Bode plots |
| **Supporting** | General Nyquist criterion (counting RHP poles); conditional stability; delay margin; why good *g_m*/φ_m can still hide a small *s_m*; minimum phase vs nonminimum phase; Bode's gain–phase relation (qualitative) |
| **Advanced / postpone** | Argument principle proof; Bode's relation as an integral formula; system gain and signal norms; passivity; small gain theorem; circle criterion; describing functions |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Complex numbers:** plotting *L(iω)* in the complex plane; distances such as *\|1 + L(iω)\|*.
- **Complex variables** (only for the general Nyquist criterion): analytic functions, poles and zeros of complex functions, the argument principle. [Stated, Preface 1st ed.]
- **Ch. 9 Bode plot.** Margins are mostly read from it in practice.

### Terms that need clarification
| Term | Issue |
|---|---|
| "Loop gain" | Sometimes \|*L(iω)*\| at a frequency, sometimes the gain factor *k* in *L*. Context decides. |
| Encirclement direction | Clockwise vs counterclockwise matters in the general criterion. Fix your convention before reading Theorem 10.3. |
| Critical point −1 vs +1 | Nyquist used +1; Bode changed it to −1 (the modern standard). [Stated §10.6] |
| Stability margin *s_m* | Not the same as the gain or phase margin. In Ch. 12–13 it appears as *1/M_s*, the inverse of the peak sensitivity. |
| Minimum phase | Defined as "no RHP zeros, no RHP poles, no delays". Some texts define it slightly differently. |
| Open loop vs loop transfer function | "Open loop" can mean the process alone, *P*, or the broken loop, *L = PC*. Here it is *L*. |

### Topics the chapter does not explain enough
- **Hand-sketching Nyquist plots** is intentionally de-emphasized. [Stated, Preface 1st ed.] Use MATLAB (`nyquist`, `margin`) and focus on interpretation.
- **Why the margins are good robustness measures**: §10.3 motivates them; the rigorous link to model uncertainty is in Ch. 13.
- **Bode's integral relations** are stated without full proof.
- **Describing function accuracy**: it is an approximation, and the book notes it can be wrong. Error bounds are not given.
- **MIMO Nyquist** (multivariable loops) is not covered.

### Note for Drillbotics [Inferred]
- **Phase margin and delay margin** are the practical numbers to check before running a WOB/RPM loop on real hardware: actuator lag, filtering, and communication delay all eat phase margin.
- **Describing functions** (Module E) are a classical tool for predicting **stick-slip-like limit cycles** caused by nonlinear friction in a feedback loop. Revisit them if stick-slip handling becomes a focus.
