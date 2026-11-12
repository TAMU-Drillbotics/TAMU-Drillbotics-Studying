# Chapter 13: Robust Performance — Layer 1 Knowledge Map

> Source: Åström & Murray, *Feedback Systems*, 2nd ed., Chapter 13, pp. 13-1 to 13-30.
> Layer: 1 (knowledge map).
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **robustness**: whether a feedback system stays **stable** and keeps **good performance** when the real process differs from the model. The chapter describes how to *model uncertainty* (parameter variations and neglected dynamics), how to test *robust stability* and *robust performance* with generalizations of the Nyquist criterion, and briefly how to *design* for robustness. The book calls it "only an introduction to … a vast topic". [Stated, chapter intro]

### Why it matters
"Robustness to uncertainty" was named in Ch. 1 as *the* key use of feedback, and Black's epigraph is the original example. Robustness is "what makes it possible to design feedback systems based on strongly simplified models". [Stated §13.1] This chapter makes that claim *quantitative*.

### Position in the book
The book says this chapter "combine[s] the results from the second half of the book to analyze some of the fundamental trade-offs between robustness and performance" and that it is "a key chapter … serving as an introduction for more advanced studies". [Stated, Preface 1st ed.]

### Major components
1. Modeling uncertainty: parametric, unmodeled dynamics, similarity of systems, the Vinnicombe metric (§13.1) ★
2. Stability in the presence of uncertainty: robust stability via Nyquist, Youla parameterization (§13.2) ★
3. Performance in the presence of uncertainty: disturbances, reference response (§13.3)
4. Design for robust performance: QFT, linear quadratic control, H∞ control, generalized stability margin, limits of robust design (§13.4)

### After this chapter you should be able to:
- Represent uncertainty as parameter ranges, or as additive / multiplicative / feedback uncertainty blocks.
- Explain why open loop similarity does not imply closed loop similarity, and the reverse.
- Derive and apply the robust stability condition for multiplicative uncertainty: *\|T(iω)·δ(iω)\| < 1* for all ω.
- Explain why *M_s* and *M_t* (peaks of *\|S\|*, *\|T\|*) are robustness measures.
- Explain how *S* and *T* change when the process changes (the sensitivity of the closed loop).
- Describe at a high level what H∞ control and the generalized stability margin aim to do.

---

## 2. Knowledge Structure

### Module A — Modeling uncertainty (§13.1) ★
**Purpose:** Describe *what we don't know* about the process in a form usable for analysis. [Stated §13.1]

| # | Concept | One-line gloss |
|---|---|---|
| 1 | Parametric uncertainty | Known model structure, uncertain parameter values (e.g., car mass, gear). Explore it by computing the response for parameter ranges. [Stated §13.1, Ex. 13.1] |
| 2 | Unmodeled dynamics | Dynamics left out of the model (e.g., fast actuator or sensor dynamics). [Stated §13.1] |
| 3 | Additive uncertainty *P + Δ* | The true process = model + an error term. [Stated §13.1, Fig. 13.2a] |
| 4 | Multiplicative uncertainty *P(1 + δ)* | Relative error. It is usually small at low frequency and large at high frequency. [Stated §13.1, Fig. 13.2b] |
| 5 | Feedback uncertainty | Uncertainty entering as a feedback around *P*. [Stated §13.1, Fig. 13.2c] |
| 6 | "When are two systems similar?" | Open loop similarity ≠ closed loop similarity (Ex. 13.2 and 13.3). [Stated §13.1] |
| 7 | Comparing frequency responses | A winding number condition is also needed (Ex. 13.4). [Stated §13.1] |
| 8 | **Definition 13.1**: Vinnicombe ν-gap metric δ_ν(P₁, P₂) | A distance between two systems that reflects closed loop behavior. [Stated §13.1] |

**Prerequisites:** Ch. 3 §3.1 (model uncertainty, qualitative); Ch. 9 (transfer functions, frequency response); Ch. 10 (Nyquist, winding number).

---

### Module B — Robust stability (§13.2) ★
**Purpose:** Guarantee stability for *all* processes in the uncertainty set. [Stated §13.2]

| Concept | One-line gloss |
|---|---|
| Robust stability via Nyquist | If the nominal loop is stable and the perturbed Nyquist curves never reach −1, the perturbed loops stay stable. [Stated §13.2] |
| Geometric condition | The uncertainty disk around *L(iω)* (radius *\|C·Δ\|*) must not contain −1, and the distance to −1 is *\|1 + L\|*. [Stated §13.2] |
| Robust stability for multiplicative uncertainty | *\|δ(iω)\| < 1/\|T(iω)\|* for all ω. [Stated §13.2] |
| Robust stability for additive uncertainty | *\|Δ(iω)\| < 1/\|C(iω)S(iω)\|*. [Stated §13.2: the corresponding additive condition] |
| *M_s, M_t* as robustness measures | Small peaks give large allowable uncertainty. [Stated §13.2] |
| Sector-bounded nonlinearities | The same reasoning covers a static nonlinearity in the loop (circle-criterion style). [Stated §13.2, Ex. 13.8] |
| Bode's ideal loop transfer function | *L = (s/ω_gc)^{−n}* with non-integer *n*: the phase margin is constant regardless of gain. [Stated §13.2, Ex. 13.9] |
| Youla parameterization | A formula describing *all* controllers that stabilize a given process, in terms of a free stable transfer function *Q*. [Stated §13.2] |

**Example:** cruise control (13.7).

**Prerequisites:** Module A; Ch. 10 (Nyquist, stability margin, small gain theorem, circle criterion); Ch. 12 (*S*, *T*, *M_s*, *M_t*).

---

### Module C — Performance under uncertainty (§13.3)
**Purpose:** Check that disturbance rejection and tracking stay good when the process changes. [Stated §13.3]

| Concept | One-line gloss |
|---|---|
| Disturbance attenuation, *G_yv = PS* | A more detailed measure than *S* alone. [Stated §13.3] |
| Reference response, *G_yr = TF* | How tracking depends on the closed loop *T* and the feedforward *F*. [Stated §13.3, eq. 13.22] |
| Sensitivity of *T* to process changes | *dT/T = S · dP/P*: the closed loop is insensitive where *\|S\|* is small. This is the origin of the name "sensitivity function". [Stated §13.3] |
| Op-amp robust performance | The amplifier gain is insensitive to op-amp gain variations, thanks to feedback (Black's idea). [Stated §13.3, Ex. 13.10] |

**Prerequisites:** Module B; Ch. 12 (Gang of Six, specs).

---

### Module D — Design for robust performance (§13.4)
**Purpose:** An overview of design methods that explicitly target robustness. [Stated §13.4]

| Concept | One-line gloss |
|---|---|
| Requirements on (at least) six transfer functions | Load rejection, noise injection, tracking, insensitivity: specs on *S, T, G_yv, G_uw, G_yr, G_ur*. [Stated §13.4] |
| Quantitative Feedback Theory (QFT) | Horowitz's graphical robust loop-shaping method. [Stated §13.4] |
| Linear quadratic control (frequency view) | Minimize a cost that trades load disturbance attenuation against noise injection. [Stated §13.4] |
| H∞ control | Minimize the H∞ norm (peak gain over frequency) of a matrix of closed loop transfer functions. [Stated §13.4] |
| Generalized stability margin *b_{P,C}* | A margin that covers the whole Gang of Four. [Stated §13.4] |
| **Theorem 13.1** (Vinnicombe's robustness theorem) | If *C* gives margin *b* for *P*, it stabilizes every process within ν-gap distance < *b* of *P*. [Stated §13.4] |
| Disturbance weighting | Frequency weights on signals to express where performance matters. [Stated §13.4] |
| Limits of robust design | Some process variations are too large for any single linear controller. Then gain scheduling or adaptation is needed. [Stated §13.4; adaptation is in Ch. 15] |

**Example:** AFM nanopositioning (13.11).

**Prerequisites:** Modules A–C; Ch. 7 §7.5 (LQR); Ch. 10 §10.5 (norms/gain); Ch. 12 (loop shaping).

---

## 3. Dependency Map

```
Ch. 3 §3.1 (uncertainty, qualitative)  Ch. 10 (Nyquist, s_m, small gain)  Ch. 12 (S, T, M_s, M_t, Gang of Six)
              └───────────────────────────────┬───────────────────────────────────┘
                                              ▼
Module A: UNCERTAINTY MODELS
  parametric → unmodeled dynamics → additive / MULTIPLICATIVE / feedback forms
  → "when are two systems similar?" → ν-gap metric (Def 13.1)
                                              │
                                              ▼
Module B: ROBUST STABILITY
  Nyquist + uncertainty disk → |δ| < 1/|T|  (and additive version) → M_s, M_t as margins
  → sector nonlinearities → Bode's ideal loop → Youla parameterization
                                              │
                                              ▼
Module C: ROBUST PERFORMANCE  (G_yv = PS, G_yr = TF, dT/T = S·dP/P)
                                              │
                                              ▼
Module D: DESIGN  (QFT; LQ; H∞; generalized margin + Vinnicombe thm (needs ν-gap from A);
                   disturbance weighting; limits → Ch. 14, Ch. 15 adaptation)
```

**Must be understood first:** multiplicative uncertainty → the robust stability condition *\|δ\| < 1/\|T\|* → *M_s/M_t* as robustness measures.

**Can be learned independently:**
- The ν-gap metric (A8) and Theorem 13.1 (D) form a separate, advanced branch. The core robust stability condition does not need them. [Inferred]
- The Youla parameterization is a side branch within Module B.
- In Module D, QFT, LQ, and H∞ are alternative methods. Each can be read alone.

**Depend on several earlier concepts:**
- The robust stability condition (B) needs multiplicative uncertainty (A), the Nyquist criterion (Ch. 10), **and** *T* (Ch. 12).
- *dT/T = S·dP/P* (C) needs *S* and *T* (Ch. 12) **and** the uncertainty idea (A).
- Vinnicombe's theorem (D) needs the ν-gap (A) **and** the generalized stability margin (D).
- H∞ (D) needs system gain / norms (Ch. 10 §10.5) **and** the Gang of Four (Ch. 12).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Parametric vs unmodeled-dynamics uncertainty; multiplicative uncertainty *P(1+δ)*; robust stability *\|δ\| < 1/\|T\|*; *M_s* and *M_t* as robustness measures; sensitivity of the closed loop *dT/T = S·dP/P*; open loop similarity ≠ closed loop similarity |
| **Supporting** | Additive and feedback uncertainty forms; additive robust stability condition; *G_yv = PS*; uncertainty disk picture on the Nyquist plot; the "limits of robust design" idea; H∞ as "minimize the worst-case gain" |
| **Advanced / postpone** | ν-gap metric and Vinnicombe's theorem; generalized stability margin; Youla parameterization; Bode's ideal loop transfer function; QFT; disturbance weighting; sector-bounded nonlinearities |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Ch. 10 and Ch. 12 are essential.** This chapter is built on the Nyquist plot and the sensitivity functions.
- **Complex magnitude inequalities** (triangle inequality *\|a + b\| ≥ \|a\| − \|b\|*), used in the robust stability derivations.
- **Norms** (H∞ norm = peak gain over frequency). They appear briefly in §10.5 and are used here.

### Terms that need clarification
| Term | Issue |
|---|---|
| Robust stability vs robust performance | The first: stable for all plants in the set. The second: specs met for all plants in the set. Robust performance is stronger. |
| δ vs Δ | δ = relative (multiplicative) error; Δ = absolute (additive) error. |
| "Similar" systems | Means similar *in closed loop*, which is what the ν-gap captures. |
| Generalized stability margin | A different number from the classic *s_m* of §10.3; it accounts for all four Gang-of-Four transfer functions. |
| H∞ | "H-infinity": the space of stable transfer functions with a bounded peak gain; also the name of the norm. |
| Youla / Q-parameterization | Expresses all stabilizing controllers with one free stable transfer function *Q*. |

### Topics the chapter does not explain enough
- **The book says it gives "only an introduction"**. [Stated, chapter intro] Robust control (μ-analysis, structured uncertainty, full H∞ synthesis) needs dedicated texts. See §13.5.
- **How to obtain the uncertainty bound δ(ω)** for a real process is not covered systematically. In practice it comes from experiments or from comparing high- and low-fidelity models.
- **H∞ synthesis algorithms** (Riccati-based) are only described conceptually.
- **Youla parameterization** is stated with little design guidance.
- **Robustness of state space designs** (e.g., LQG margins) is not analyzed explicitly.

### Note for Drillbotics [Inferred]
- The team plan's sequence **sim → toy drill → lab rig** is a robustness problem: the same controller has to work on three different "plants". Module A's multiplicative uncertainty and Module B's *\|δ\| < 1/\|T\|* give you a way to check this *before* hardware testing: compare the Simscape model with simplified models across frequency.
- **Unmodeled dynamics** (drillstring torsional modes, actuator lag) are the typical cause of loops that work in simulation and fail on hardware. Keep the bandwidth below where δ(ω) gets large.
