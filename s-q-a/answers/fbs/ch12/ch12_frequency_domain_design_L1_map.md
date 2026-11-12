# Chapter 12: Frequency Domain Design — Layer 1 Knowledge Map

> Source: Åström & Murray, *Feedback Systems*, 2nd ed., Chapter 12, pp. 12-1 to 12-32.
> Layer: 1 (knowledge map).
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **how to design feedback (and feedforward) controllers in the frequency domain**. It starts with a full description of **performance specifications** (through the sensitivity functions), then **loop shaping** (designing *L = PC* to have a good shape), **feedforward design**, the **root locus** method, and a nested-loop **design example**. [Stated, chapter intro]

### Position in the book
- Ch. 10 used *L = PC* to *analyze* stability. Ch. 12 uses it to *design*. [Stated §12.1, §12.3]
- The book says this chapter "introduces many of the ideas of modern control theory, including the sensitivity function". [Stated, Preface 1st ed.]
- It generalizes PID (Ch. 11) to arbitrary compensators. [Stated §12.1]

### What problems does it address?
- *L* alone gives "only limited insight" into the closed loop. What *other* transfer functions must we check? (§12.1: Gang of Four / Gang of Six)
- How do we write precise specifications for tracking, disturbance rejection, noise, and robustness? (§12.2)
- How do we choose a controller that gives *L* the right shape? (§12.3: loop shaping, lead/lag)
- How do we add feedforward correctly, especially when the process has RHP zeros? (§12.4)
- What can a controller of *limited complexity* achieve? (§12.5: root locus)

### The epigraph's warning (Gunter Stein) [Stated]
"Sensitivity improvements in one frequency range must be paid for with sensitivity deteriorations in another frequency range." This trade-off is previewed here and proved in Ch. 14.

### Major components
1. Sensitivity functions: the Gang of Four / Six (§12.1) ★
2. Performance specifications: reference response, load disturbances, measurement noise, measuring specs (§12.2) ★
3. Feedback design via loop shaping: design considerations, lead and lag compensation (§12.3) ★
4. Feedforward design: combining it with feedback, difficulties, approximate inverses (§12.4)
5. The root locus method (§12.5)
6. Design example: lateral control of a vectored thrust aircraft (§12.6)

### After this chapter you should be able to:
- Write the Gang of Four: *S = 1/(1+PC)*, *T = PC/(1+PC)*, *PS*, *CS*. State what each one describes, and that *S + T = 1*.
- Express specs (overshoot, bandwidth, disturbance attenuation, noise injection, robustness via *M_s*, *M_t*) in terms of these transfer functions.
- Describe the "good shape" of *L(iω)*: high gain at low frequency, crossover with an adequate phase margin, roll-off at high frequency.
- Use lead and lag compensators to modify *L*.
- Design a feedforward controller *F = F_m/P* and explain why RHP zeros and delays make exact inversion impossible.
- Sketch and interpret a root locus.

---

## 2. Knowledge Structure

### Module A — Sensitivity functions (§12.1) ★
**Purpose:** Show that *L* alone is not enough. The closed loop must be judged by several transfer functions. [Stated §12.1, Ex. 12.1]

| Concept | One-line gloss |
|---|---|
| Basic feedback loop with *P, C, F* and inputs *r, v, w* | Reference, load disturbance, measurement noise. [Stated §12.1, Fig. 12.1] |
| Gang of Six | All six transfer functions from *(r, v, w)* to *(y, u)*. With *F = 1* they reduce to four. [Stated §12.1–12.2] |
| Gang of Four | *S = 1/(1+PC)* (sensitivity), *T = PC/(1+PC)* (complementary sensitivity), *PS* (load sensitivity), *CS* (noise sensitivity). [Stated §12.1] |
| *S + T = 1* | You cannot make both small at the same frequency. [Stated §12.1] |
| Why *L* is not enough | Ex. 12.1: an unstable pole canceled by the controller looks fine in *L* but shows up in *PS*. [Stated §12.1] |

**Prerequisites:** Ch. 9 §9.4 (block diagram algebra, closed loop transfer functions); Ch. 10 (*L = PC*, stability).

---

### Module B — Performance specifications (§12.2) ★
**Purpose:** Turn requirements into conditions on the Gang of Six and *L*. [Stated §12.2]

| Concept | One-line gloss |
|---|---|
| Robustness specs | Margins (§10.3); maximum sensitivity *M_s = max\|S(iω)\|*; *M_t = max\|T(iω)\|*. *s_m = 1/M_s*. [Stated §12.2] |
| Reference response specs | From *G_yr = PCF/(1+PC)* and *G_ur = CF/(1+PC)*: overshoot, rise time, settling time, bandwidth, resonant peak; also the control effort. [Stated §12.2] |
| Load disturbance attenuation | Compare closed loop with open loop: *S* is the attenuation factor. Disturbances are attenuated at frequencies where *\|S\| < 1* and amplified where *\|S\| > 1*. [Stated §12.2] |
| Measurement noise | *CS* tells how much noise reaches the control signal. High controller gain at high frequency causes actuator wear. [Stated §12.2] |
| Measuring specifications | Specs can be checked by computing the Gang of Six numerically, or by experiments that inject signals at the right points. [Stated §12.2] |

**Examples:** reference tracking (12.2) and disturbance attenuation (12.3) for *P = 1/(s+1)³*.

**Prerequisites:** Module A; Ch. 6 (step and frequency response specs); Ch. 10 (margins).

---

### Module C — Feedback design via loop shaping (§12.3) ★
**Purpose:** Choose *C* so that *L = PC* has a desired shape. [Stated §12.3]

| Concept | One-line gloss |
|---|---|
| Loop shaping | A family of methods: pick a compensator that gives *L* a desired shape. "Bend the Nyquist curve away from −1." [Stated §12.3] |
| Desired loop shape | **Large gain at low frequency** (tracking and disturbance rejection), **adequate phase margin at crossover** (robustness), **small gain at high frequency** (noise and unmodeled dynamics). [Stated §12.3, Fig. 12.8] |
| Gain crossover frequency ω_gc as the main design parameter | It sets the bandwidth (speed) and is limited by the process (Ch. 14). [Inferred from §12.3; formalized in §14.3] |
| Lead compensator | Adds phase near crossover: more phase margin, faster response. [Stated §12.3] |
| Lag compensator | Adds low-frequency gain: smaller steady-state error, with little effect at crossover. [Stated §12.3] |
| PI and PD as lag and lead | PI is a lag-type compensator; PD is a lead-type compensator. [Stated §12.3] |

**Examples:** AFM in tapping mode (12.4), roll control of a vectored thrust aircraft (12.5).

**Prerequisites:** Modules A and B; Ch. 9 Bode plot; Ch. 10 margins and Bode's gain–phase relation.

---

### Module D — Feedforward design (§12.4)
**Purpose:** Improve reference response and reject *measured* disturbances. Feedforward "complements feedback". [Stated §12.4]

| Concept | One-line gloss |
|---|---|
| Combining feedforward and feedback | 2DOF structure: feedforward from *r* (via a reference model *F_m*) and from a measured disturbance *v*; feedback handles the rest. [Stated §12.4, Fig. 12.13] |
| Ideal feedforward | *F_r = F_m/P*, *F_v = 1/P₁*: invert the process. [Stated §12.4, eq. 12.13] |
| Difficulties | RHP zeros → unstable inverse; time delays → non-causal inverse; high relative degree → large control signals. [Stated §12.4] |
| **Theorem 12.1** (approximate inverse) | Factor *G = G₊G₋*. Invert only the "good" part and reflect the RHP zeros. [Stated §12.4, presented without proof] |
| Links to earlier feedforward | §8.5 (state space feedforward), §11.5 (setpoint weighting). [Stated §12.4] |

**Examples:** vehicle steering (12.6), RHP zero (12.7, 12.8).

**Prerequisites:** Module A; Ch. 1 §1.2 (feedforward idea); Ch. 9 (RHP zeros, time delays); Ch. 8 §8.5 (2DOF).

---

### Module E — The root locus method (§12.5)
**Purpose:** Study what a *restricted-complexity* controller can do by plotting the closed loop poles as one gain varies. [Stated §12.5]

| Concept | One-line gloss |
|---|---|
| Restricted complexity | When *C* is simpler than *P*, not all closed loop poles can be placed (contrast with Ch. 7–8). [Stated §12.5] |
| Root locus | The paths of the closed loop poles in the complex plane as a gain *k* goes from 0 to ∞. [Stated §12.5] |
| Start/end points | Starts at the open loop poles and ends at the open loop zeros (or at infinity along asymptotes). [Stated §12.5] |
| Use in design | Choose *k* to get dominant poles with the desired ζ and ω₀. [Inferred: standard use; the book presents root locus mainly as a "hook"] |

**Prerequisites:** Ch. 5 §5.5 (root locus first appearance); Ch. 7 (eigenvalue assignment); Ch. 9 (poles and zeros).

**Priority note:** The book de-emphasizes root locus and adds it "to at least serve as hooks". [Stated, Preface 2nd ed. and 1st ed.]

---

### Module F — Design example (§12.6)
**Purpose:** Combine the techniques in one realistic design. [Stated §12.6]

| Concept | One-line gloss |
|---|---|
| Nested (inner/outer) loop design | Inner loop: roll/attitude. Outer loop: lateral position. Design the inner loop first, then treat it as part of the process for the outer loop. [Stated §12.6; inner/outer structure from Ex. 12.9] |
| Bandwidth separation | The inner loop must be much faster than the outer loop. [Inferred: standard cascade rule, consistent with §1.5's cascaded loops] |

**Example:** lateral control of a vectored thrust aircraft (12.9).

**Prerequisites:** Modules A–C; Ch. 1 §1.5 (cascaded loops / modularity).

---

## 3. Dependency Map

```
Ch. 9: block-diagram algebra, Bode plot      Ch. 10: L = PC, Nyquist, margins, Bode gain–phase
Ch. 11: PID                                   Ch. 6: time/frequency response specs
        └──────────────────┬──────────────────────┘
                           ▼
Module A: GANG OF FOUR / SIX  (S, T, PS, CS; S + T = 1; why L alone is not enough)
                           │
                           ▼
Module B: SPECIFICATIONS  (robustness M_s/M_t; reference; load disturbance via S; noise via CS)
                           │
          ┌────────────────┼─────────────────────┬──────────────────┐
          ▼                ▼                     ▼                  ▼
Module C: LOOP SHAPING   Module D: FEEDFORWARD  Module E:          (Ch. 13: robust
desired L shape →        ideal inverse →        ROOT LOCUS         performance builds
lead / lag               difficulties (RHP      (needs Ch. 5, 7,   on A + B)
                         zeros, delays) →       9 poles/zeros)
                         approximate inverse
          └────────────────┬─────────────────────┘
                           ▼
Module F: DESIGN EXAMPLE (nested loops; needs A–C, Ch. 1 cascade idea)
```

**Must be understood first:** the Gang of Four (especially *S* and *T*) → what each one controls → the desired shape of *L*.

**Can be learned independently:**
- Module E (root locus) needs only poles and zeros (Ch. 9) and the eigenvalue ideas (Ch. 5, 7). It does not depend on A–D. [Inferred]
- Module D (feedforward) needs Module A but not Module C.
- Lead and lag compensation (in C) are separate tools that can be learned one at a time.

**Depend on several earlier concepts:**
- Loop shaping (C) needs the specs (B), Bode plots (Ch. 9), margins (Ch. 10), **and** Bode's gain–phase relation (Ch. 10 §10.4).
- The approximate inverse (D) needs RHP zeros (Ch. 9), feedforward (Ch. 1, 8), **and** Theorem 12.1.
- The design example (F) needs loop shaping (C) **and** cascaded loops (Ch. 1).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Gang of Four (*S, T, PS, CS*); *S + T = 1*; *M_s* as a robustness measure; disturbance attenuation where *\|S\| < 1*; noise reaching *u* via *CS*; desired loop shape (high gain at low ω, good phase margin at crossover, roll-off at high ω); lead and lag compensators; feedforward = approximate process inverse |
| **Supporting** | Gang of Six with *F ≠ 1*; reference specs via *G_yr*; why *L* alone can hide an unstable cancellation; difficulties of feedforward (RHP zeros, delays); nested loop design and bandwidth separation |
| **Advanced / postpone** | Theorem 12.1 (approximate inverse) details; root locus rules; measuring specs experimentally; the full vectored thrust aircraft example |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Ch. 9 Bode plots and Ch. 10 margins** are used constantly.
- **Complex magnitude:** *\|S(iω)\| = 1/\|1 + L(iω)\|* = 1 / (distance from *L(iω)* to −1). This geometric fact connects *M_s* to the Nyquist plot.
- **Basic frequency-domain intuition:** "low frequency" = slow signals, constant disturbances; "high frequency" = noise, unmodeled fast dynamics.

### Terms that need clarification
| Term | Issue |
|---|---|
| Sensitivity function *S* | Named for Bode's sensitivity: the relative change of the closed loop for a relative change of the process. It is also the transfer function from *r* to *e*. |
| Complementary sensitivity *T* | "Complementary" because *S + T = 1*. |
| *M_s*, *M_t* | Peak values (maxima over frequency) of *\|S\|* and *\|T\|*. |
| Lead / lag | Named for the phase they add: lead = positive phase, lag = negative phase near their break frequencies. |
| *F_m* (reference model) | The desired closed loop response to *r*. |
| "Gang of Four" | The book's own name; other texts list *S, T, PS, CS* without the name. |

### Topics the chapter does not explain enough
- **Systematic loop shaping algorithms** (e.g., H∞ loop shaping) are only referenced. The chapter teaches the manual, intuition-based version.
- **Root locus construction rules** are de-emphasized. [Stated, Preface 1st ed.]
- **Proof of Theorem 12.1** is not given. [Stated §12.4: "presented without proof"]
- **Why** improving *S* in one band worsens it elsewhere: only quoted (Stein epigraph). The proof is Bode's integral, §14.2.
- **Discrete-time loop shaping**: not covered.

### Note for Drillbotics [Inferred]
- **Feedforward from a measured disturbance (Module D)** is directly relevant: if the bit model predicts torque changes (e.g., from WOB changes or formation changes), feedforward can counteract them before the RPM loop sees an error.
- **Nested loops (Module F)**: a motor current/torque loop inside an RPM loop inside a ROP/WOB loop has exactly this structure. Design inner loops first and keep their bandwidths well separated.
- **Noise sensitivity *CS* (Module B)** tells you how much sensor noise (e.g., from vibration) reaches your motor commands.
