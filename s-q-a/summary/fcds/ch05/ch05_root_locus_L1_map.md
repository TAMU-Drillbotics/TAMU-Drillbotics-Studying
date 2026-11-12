# Chapter 5: The Root-Locus Design Method — Layer 1 Knowledge Map

> Source: Franklin, Powell & Emami-Naeini, *Feedback Control of Dynamic Systems*, 7th ed., Chapter 5, pp. 234–307.
> Layer: 1 (knowledge map). What to learn and in what order, not full explanations.
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **how the closed-loop poles move in the s-plane as one parameter (usually the loop gain *K*) changes**. The plot of those paths is the **root locus**, introduced by W. R. Evans. [Stated, Perspective] Because Ch. 3 tied pole locations to transient response, the root locus shows directly how a gain change alters the response.

### What problems does it address?
- Choosing a gain that gives acceptable damping and speed.
- Seeing when a gain makes the loop unstable.
- Deciding what **dynamic compensation** (lead, lag, notch) to add when a gain alone cannot meet the specs. [Stated, Chapter Overview]
- Sanity-checking Matlab's root-locus plots by knowing the rules. [Stated, Perspective]

### Major components
1. Definition: root locus of 1 + *KL*(*s*) = 0 (§5.1)
2. Rules for sketching a positive (180°) locus, and choosing *K* (§5.2) ★
3. Illustrative loci: effect of extra poles, zeros, flexibility (§5.3)
4. Dynamic compensation: lead, lag, notch (§5.4) ★
5. A full design example: small-airplane autopilot (§5.5)
6. Extensions: negative (0°) locus, two parameters, time delay (§5.6)

### After this chapter you should be able to:
- Put any characteristic equation into the form 1 + *KL*(*s*) = 0.
- Sketch a root locus by hand using Rules 1–3 (and 4 when a pole is near the *j*ω axis), and verify it with `rlocus`.
- Read off the gain *K* for a chosen closed-loop pole location.
- Explain what lead and lag compensators do to a locus, and when to use a notch.
- Recognize a conditionally stable system.

---

## 2. Knowledge Structure

### Module A — What a root locus is (§5.1)
**Purpose:** Set up the problem in a standard form.

| Concept | One-line gloss |
|---|---|
| Closed-loop transfer function | *T*(*s*) = *D*꜀*G*/(1 + *D*꜀*GH*) for the loop of Fig. 5.1 (with sensor *H*). [Stated §5.1] |
| Characteristic equation | 1 + *D*꜀*GH* = 0; its roots are the closed-loop poles. [Stated §5.1] |
| Root-locus form | Rewrite as *a*(*s*) + *Kb*(*s*) = 0, i.e., 1 + *KL*(*s*) = 0 with *L* = *b*/*a*. [Stated §5.1] |
| Root locus | The set of all roots of 1 + *KL*(*s*) = 0 as the real parameter *K* goes from 0 to ∞. [Stated §5.1, Summary] |
| Any parameter | *K* need not be a gain: any real parameter that enters the equation **linearly** works (e.g., a sensor gain, motor inertia). [Stated, Perspective] |
| Breakaway / break-in points | Where branches leave / join the real axis (multiple roots). [Stated §5.1] |

**Why together:** These define *what* is being plotted.

**Prerequisites:** Ch. 3 (poles, characteristic equation, s-plane ↔ response); Ch. 4 (closed-loop equations).

---

### Module B — Rules for a positive (180°) locus (§5.2) ★
**Purpose:** Sketch the locus quickly, by hand, from the poles and zeros of *L*(*s*).

Learn these in order:

| # | Concept | One-line gloss |
|---|---|---|
| 1 | Phase condition | *s* is on the locus (for *K* > 0) iff ∠*L*(*s*) = 180° (mod 360°). This is the second definition of the locus. [Stated §5.2, Summary] |
| 2 | Rule 1 | The *n* branches start at the poles of *L*; *m* of them end at the zeros of *L*; the rest go to infinity. [Stated §5.2.1] |
| 3 | Rule 2 | On the real axis, the locus lies to the left of an **odd** number of real poles + zeros. [Stated §5.2.1] |
| 4 | Rule 3 | *n* − *m* branches approach asymptotes centered at α = (Σ*p*ᵢ − Σ*z*ᵢ)/(*n* − *m*), at angles (180° + 360°(*l* − 1))/(*n* − *m*). [Stated §5.2.1, Summary] |
| 5 | Rule 4 | Angles of departure from poles and arrival at zeros, from the phase condition. Important when a pole is near the *j*ω axis. [Stated §5.2.1, Review Q 5.9] |
| 6 | Rule 5 | Multiple roots (breakaway points) and the angles there. [Stated §5.2.1] |
| 7 | Magnitude condition: selecting *K* | At a chosen point *s*₀ on the locus, *K* = 1/\|*L*(*s*₀)\| (measure distances to poles and zeros). [Stated §5.2.3, Summary] |
| 8 | Which rules to memorize | It is usually enough to use "Rules 1 to 3, which should be memorized." Rule 4 is sometimes useful, especially for a pole near the *j*ω axis. [Stated §5.2.1] |
| 9 | Matlab | `rlocus`, `rlocfind`, and `rltool`. [Stated, Summary] |

**Why together:** All rules come from the single phase condition (#1). [Stated §5.2.1]

**Prerequisites:** Module A; complex numbers (the angle of a product/quotient = sum/difference of angles).

---

### Module C — Illustrative loci (§5.3)
**Purpose:** Build intuition for how poles and zeros shape a locus.

| Concept | One-line gloss |
|---|---|
| Satellite (1/*s*²) with PD | Adding a zero pulls the locus into the LHP (Ex. 5.3). [Stated §5.3] |
| Satellite with lead | A zero plus a far pole: almost PD, but realizable (Ex. 5.4). [Stated §5.3] |
| Effect of a third pole | As an extra pole moves, break-in and breakaway points appear and disappear (Ex. 5.5–5.7). [Stated §5.3] |
| Collocated flexibility | Locus stays well-behaved (Ex. 5.8). [Stated §5.3] |
| Noncollocated flexibility | Locus branches head into the RHP; much harder to control (Ex. 5.9). [Stated §5.3] |
| Conditionally stable system | Stable only for a range of gains; **reducing** the gain can destabilize it (Review Q 5.10–5.11). [Stated, Review Questions] |

**Prerequisites:** Module B; Ch. 2 (collocated vs noncollocated).

---

### Module D — Dynamic compensation (§5.4) ★
**Purpose:** When adjusting *K* alone is not enough, add poles and zeros to reshape the locus.

| Concept | One-line gloss |
|---|---|
| Lead compensation | *D*꜀(*s*) = *K*(*s* + *z*)/(*s* + *p*) with *z* < *p*. Approximates PD; moves the locus left and improves damping. [Stated §5.4.1, Summary] |
| Lag compensation | Same form with *z* > *p*. Approximates PI; raises low-frequency gain to reduce steady-state error, with slight loss of stability. [Stated §5.4.2, Summary] |
| Notch compensation | A compensator whose zeros sit near a lightly damped plant resonance, so that the resonance does not go unstable when the loop is closed. [Stated §5.4.3; Ch. 10 §10.2 uses it on a satellite] |
| Why not pure PD | Pure derivative amplifies high-frequency noise; lead adds a pole to limit that. [Inferred, consistent with Ch. 6 §6.7] |
| Analog/digital implementation | Web only (W5.4.4). [Stated §5.4.4] |

**Prerequisites:** Module B (to see the effect); Ch. 4 (PD and PI, system type).

---

### Module E — A design example (§5.5)
**Purpose:** Show a complete root-locus design: autopilot for a small airplane (Piper Dakota, Ex. 5.12). [Stated §5.5]

| Concept | One-line gloss |
|---|---|
| Design flow | Specs → s-plane region → try gain → add compensation → check response. [Inferred from §5.5] |
| Successive loop closure | Close an inner loop first, then design the outer loop on the result; the root locus handles each loop in turn. [Stated, Summary] |

**Prerequisites:** Modules B and D; Ch. 3 specs.

---

### Module F — Extensions (§5.6)
| Concept | One-line gloss |
|---|---|
| Negative (0°) root locus | For *K* < 0, the phase condition becomes ∠*L* = 0°; the rules change accordingly. [Stated §5.6.1, Summary] |
| Two parameters (▲) | Study one parameter, then the next, in succession. [Stated §5.6.2] |
| Time delay (▲, web) | A delay *e*^{−*sT*} is not a rational function; web Appendix W5.6.3. [Stated §5.6.3] |

**Prerequisites:** Module B.

---

## 3. Dependency Map

```
Ch.3: s-plane ↔ response (ζ, ωn, σ)      Ch.4: characteristic eq. 1+GDc=0, PD/PI
              │                                     │
              └──────────────┬──────────────────────┘
                             ▼
Module A: 1 + K L(s) = 0;  locus = roots vs K
                             │
                             ▼
Module B ★: phase condition ∠L = 180°
            → Rules 1–3 (memorize) → Rule 4 (departure) → Rule 5 (multiple roots)
            → magnitude condition (pick K)
                             │
          ┌──────────────────┼────────────────────┐
          ▼                  ▼                    ▼
Module C: illustrative   Module D ★: lead / lag / notch   Module F: negative locus,
loci (extra pole,             │                           two parameters, delay
collocated vs                 ▼
noncollocated)           Module E: airplane design example; successive loop closure
```

**Must be understood first:** the form 1 + *KL*(*s*) = 0; the phase condition; Rules 1–3.

**Can be learned independently:**
- Negative locus (F) and the notch filter (D) do not depend on each other.
- Module C examples can be read in any order.

**Depend on several earlier concepts:**
- Lead/lag design (D) needs the rules (B) **and** PD/PI and system type (Ch. 4) **and** s-plane specs (Ch. 3).
- Conditional stability (C) needs the locus shape (B) **and** the stability boundary (Ch. 3).
- Successive loop closure (E) needs block-diagram reduction (Ch. 3) **and** the full locus method.

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | 1 + *KL*(*s*) = 0; definition of the root locus; phase condition; Rules 1–3; magnitude condition for *K*; lead compensation; lag compensation; using `rlocus`/`rltool` |
| **Supporting** | Rule 4 (departure/arrival angles); breakaway points; illustrative loci; collocated vs noncollocated loci; notch compensation; conditionally stable systems; successive loop closure |
| **Advanced / postpone** | Rule 5 details; negative (0°) locus; two-parameter loci; time delay (web); analog/digital implementation (web); history (§5.7) |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Complex-number geometry:** the angle of (*s* − *p*) is the angle of the vector from *p* to *s*; angles of products add, quotients subtract.
- Ch. 3 s-plane regions (ζ lines, σ lines, ωₙ circles).
- Ch. 4 PID effects.

### Terms that need clarification
| Term | Issue |
|---|---|
| *L*(*s*) | The book's notation for the loop transfer function *without* the parameter *K*. Not the same as FBS's *L*(*s*), which includes all gains. |
| *n*, *m* | Number of poles and zeros of *L*(*s*). |
| "Positive" vs "negative" locus | Refers to the sign of *K*, not to positive/negative feedback directly. |
| Lead / lag | Names come from their phase (lead adds positive phase, lag negative phase). The phase meaning is clearer in Ch. 6. |

### Topics the chapter does not explain enough
- **The root locus shows closed-loop poles only.** The closed-loop zeros also shape the response (Ch. 3 §3.5); you must check them separately. [Inferred]
- **Robustness and noise** are hard to see on a root locus. Frequency response (Ch. 6) handles them better. [Inferred, consistent with Ch. 6 Perspective]
- **Time delay** is web only.
- **Detailed lead/lag design procedures** are clearer in Ch. 6 §6.7.

### Note for Drillbotics [Inferred]
- Use `rltool` / Control System Designer in MATLAB to pick the WOB or RPM loop gain against overshoot and settling specs.
- An **inverted pendulum** (proposed as a test rig) has an RHP open-loop pole. Its root locus shows why a pure P controller cannot stabilize it and why a lead/PD zero is needed.
- The **noncollocated** example (Ex. 5.9) is a warning for controlling bit behavior from surface sensors.
