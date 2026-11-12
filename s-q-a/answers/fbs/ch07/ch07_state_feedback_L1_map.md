# Chapter 7: State Feedback — Layer 1 Knowledge Map

> Source: Åström & Murray, *Feedback Systems*, 2nd ed., Chapter 7, pp. 7-1 to 7-34.
> Layer: 1 (knowledge map).
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **how to design a controller when the full state *x* is measured**: the control law *u = −Kx + k_f r*. The central idea is to **choose the closed loop eigenvalues** (and therefore the closed loop dynamics) by choosing *K*. [Stated, chapter intro and §7.2]

### What problems does it address?
- Can the input steer the state *anywhere*? (§7.1: **reachability**)
- If so, how do we choose *K* to put the closed loop eigenvalues where we want? (§7.2: **eigenvalue assignment**, also called pole placement)
- *Where* should we put them? What are the trade-offs between speed, input size, and robustness? (§7.3)
- How do we get zero steady-state error without a perfect model? (§7.4: **integral action**)
- Can we pick *K* by optimization instead of picking eigenvalues? (§7.5: **LQR**)

### Position in the book
This is the **first formal design chapter**. It turns Ch. 1's claim that feedback can "design the dynamics" into a procedure. [Inferred] It assumes the state is measured. Ch. 8 removes that assumption with an *observer*. [Stated, Preface 1st ed.: Ch. 7 and 8 introduce reachability and observability]

### Major components
1. Reachability (§7.1) ★
2. Stabilization by state feedback / eigenvalue assignment (§7.2) ★
3. Design considerations: 2nd-order specs, higher-order systems, trade-offs (§7.3)
4. Integral action (§7.4) ★
5. Linear quadratic regulators (§7.5)

### After this chapter you should be able to:
- Define reachability and test it with the rank of *W_r = [B AB … Aⁿ⁻¹B]*.
- Design *K* for a single-input system to place the closed loop eigenvalues, and choose *k_f* for zero steady-state error to a constant reference.
- Translate specs (rise time, overshoot, settling time) into a damping ratio ζ and natural frequency ω₀ for a 2nd-order system.
- Add an integrator state to remove steady-state error robustly.
- Explain LQR as trading state error against input effort through the weights *Q* and *R*.

---

## 2. Knowledge Structure

### Module A — Reachability (§7.1) ★
**Purpose:** Find out whether the input can steer the system at all. This determines whether state feedback can place every eigenvalue. [Stated §7.1]

| # | Concept | One-line gloss |
|---|---|---|
| 1 | **Definition 7.1** (reachability) | For any start *x₀* and target *x_f* there is an input that drives *x₀* to *x_f* in finite time. [Stated §7.1] |
| 2 | Reachable set | The set of states reachable from the origin. [Stated §7.1] |
| 3 | Reachability matrix *W_r = [B AB … Aⁿ⁻¹B]* | Built from *A* and *B*. [Stated §7.1] |
| 4 | **Theorem 7.1** (rank condition) | Reachable ⇔ *W_r* has full rank (is invertible for a single input). [Stated §7.1] |
| 5 | Unreachable systems | Some modes cannot be affected by the input (e.g., two identical systems driven by the same input). [Stated §7.1] |
| 6 | Reachable canonical form (**Theorem 7.2**) | A coordinate system where *A*'s last row (or first row, depending on convention) holds the characteristic polynomial coefficients. Possible only if the system is reachable. [Stated §7.1] |
| 7 | Reachability vs "controllability" | Most textbooks say *controllability*. Kalman distinguished reaching the origin from reaching an arbitrary state. [Stated §7.6] |

**Examples:** double integrator (7.1), balance system (7.2).

**Prerequisites:** Ch. 3 (*A, B* form); Ch. 6 (convolution equation, coordinate changes); **linear algebra: rank, invertibility, change of basis**.

---

### Module B — Stabilization by state feedback (§7.2) ★
**Purpose:** Design *K* so that the closed loop has chosen eigenvalues. [Stated §7.2]

| # | Concept | One-line gloss |
|---|---|---|
| 1 | Control law *u = −Kx + k_f r* | State feedback plus a reference (feedforward) gain. [Stated §7.2] |
| 2 | Closed loop dynamics *dx/dt = (A − BK)x + Bk_f r* | Feedback replaces *A* by *A − BK*. [Stated §7.2] |
| 3 | Reference gain *k_f* | Chosen so the steady-state output equals *r*. It depends on the model being exact. [Stated §7.2] |
| 4 | Eigenvalue assignment problem | Choose *K* so that *det(sI − (A − BK))* equals a desired polynomial *p(s)*. [Stated §7.2] |
| 5 | State feedback in reachable canonical form | Each gain *kᵢ* directly shifts one polynomial coefficient. [Stated §7.2] |
| 6 | **Theorem 7.3** (eigenvalue assignment) | If the system is reachable, *any* set of eigenvalues can be assigned (single input). [Stated §7.2] |
| 7 | Ackermann's formula / MATLAB `place`, `acker` | Computational tools for finding *K*. [Stated §7.2] |

**Examples:** vehicle steering (7.4), predator–prey regulation (7.5).

**Prerequisites:** Module A (reachability, canonical form); Ch. 5 (eigenvalues ↔ stability); Ch. 6 (steady-state response).

---

### Module C — Design considerations (§7.3)
**Purpose:** Decide *where* to put the eigenvalues. This is "the main design decision". [Stated §7.3]

| Concept | One-line gloss |
|---|---|
| Trade-offs | Input magnitude vs robustness vs performance. [Stated §7.3] |
| Standard 2nd-order system | Characteristic polynomial *s² + 2ζω₀s + ω₀²*. [Stated §7.3] |
| Damping ratio ζ, natural frequency ω₀ | ζ sets the shape (over-, critically, or underdamped); ω₀ sets the speed. [Stated §7.3] |
| Spec formulas (table) | Rise time, overshoot, settling time, bandwidth, resonant peak expressed in ζ and ω₀. [Stated §7.3] |
| Higher-order systems and dominant eigenvalues | Choose a dominant pair to meet the specs and place the rest faster. [Stated §7.3] |
| Input magnitude | Faster eigenvalues ⇒ larger gains ⇒ larger inputs ⇒ actuator saturation. [Stated §7.3] |

**Examples:** drug administration (7.6), balance system (7.7).

**Prerequisites:** Module B; Ch. 6 step response specs.

---

### Module D — Integral action (§7.4) ★
**Purpose:** Get zero steady-state error **without** relying on an exact *k_f* calibration. [Stated §7.4]

| Concept | One-line gloss |
|---|---|
| Problem with *k_f* | It needs an exact model. Model errors and disturbances leave an offset. [Stated §7.4] |
| Integral feedback | Add a controller state *z* with *dz/dt = y − r*, and feed it back. [Stated §7.4] |
| System augmentation | New state *(x, z)*. Design *K* for the augmented system. [Stated §7.4] |
| Reachability of the augmented system | Must be checked with a new *W_r*. [Stated §7.4] |
| Steady state ⇒ *y = r* | If the closed loop is stable, *dz/dt = 0* at steady state, which forces *y = r*. [Stated §7.4] |

**Example:** cruise control (7.8).

**Prerequisites:** Modules A and B; the integral action idea from §1.6, §2.3–2.4. [Stated §7.4 refers back to these]

---

### Module E — Linear Quadratic Regulator (§7.5)
**Purpose:** Choose *K* by optimization instead of picking eigenvalues directly. [Stated §7.5]

| Concept | One-line gloss |
|---|---|
| Quadratic cost *J = ∫(xᵀQ_x x + uᵀQ_u u) dt* | Penalizes state deviation and input effort. [Stated §7.5] |
| Weighting matrices *Q_x, Q_u* | Their relative size sets the trade-off between performance and effort. [Stated §7.5] |
| Algebraic Riccati equation | Its solution *P* gives the optimal gain *K = Q_u⁻¹BᵀP*. [Stated §7.5] |
| Multi-input systems | LQR handles several inputs naturally, while eigenvalue assignment for multiple inputs has no unique *K*. [Inferred] |
| Practical weight choice | Start from diagonal weights scaled by the allowed sizes of the variables (Bryson-style). [Inferred: the book gives diagonal-weight guidance in an example] |

**Examples:** vectored thrust aircraft (7.9), web server control (7.10, discrete time).

**Prerequisites:** Module B (state feedback structure); matrix algebra; the idea of optimization. The Riccati equation is used as a black box.

---

## 3. Dependency Map

```
Ch. 3 (A, B)   Ch. 5 (eigenvalues ↔ stability)   Ch. 6 (convolution, coordinates, step specs)
     └──────────────┬──────────────────────────────┘
                    ▼
Module A: REACHABILITY (Def 7.1) → rank test W_r (Thm 7.1) → reachable canonical form (Thm 7.2)
                    │
                    ▼
Module B: u = −Kx + k_f r → A − BK → EIGENVALUE ASSIGNMENT (Thm 7.3) → place / acker
                    │
      ┌─────────────┼───────────────────────┐
      ▼             ▼                       ▼
Module C:       Module D:               Module E:
Where to put    INTEGRAL ACTION         LQR (cost J, Q/R weights, Riccati)
eigenvalues?    (augment with z, check  (alternative to C for choosing K)
ζ, ω₀, dominant  reachability again)
poles, effort   needs A + B + Ch.1–2 integral idea
```

**Must be understood first:** reachability → *A − BK* → Theorem 7.3.

**Can be learned independently (after Module B):**
- Module C (where to place eigenvalues) and Module E (LQR) are **two alternative ways of choosing K**. Either can be learned without the other. [Inferred]
- Module D (integral action) does not need C or E.

**Depend on several earlier concepts:**
- Reachable canonical form (A6) needs reachability (A4) **and** coordinate transformations (Ch. 6).
- Integral action (D) needs eigenvalue assignment (B), reachability (A, applied to the augmented system), **and** the steady-state argument (Ch. 6).
- The 2nd-order spec table (C) needs the step response specs (Ch. 6) **and** the characteristic polynomial (Ch. 2, 5).

**Forward links [Stated, Preface and §7.6]:** Ch. 8 builds the *dual* concept (observability) and combines state feedback with an observer. That combination is *output feedback*.

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Reachability (definition and rank test); *u = −Kx + k_f r*; closed loop matrix *A − BK*; Theorem 7.3 (reachable ⇒ arbitrary eigenvalues); ζ and ω₀ for 2nd-order systems; integral action by state augmentation |
| **Supporting** | Reachable canonical form; reference gain *k_f* and its sensitivity to model error; dominant eigenvalues; input magnitude / saturation trade-off; LQR cost and the meaning of *Q, R* |
| **Advanced / postpone** | Proof of the rank condition; Ackermann's formula derivation; the Riccati equation itself; discrete-time LQR (web server example); the reachability vs controllability distinction |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Linear algebra:** matrix rank, invertibility, determinants, characteristic polynomial *det(sI − A)*, similarity transformations. [Stated, Preface: assumed]
- **Optimization basics:** what minimizing a cost function means (for LQR).
- **Ch. 6 step response specs.** Module C relies on them.

### Terms that need clarification
| Term | Issue |
|---|---|
| Reachability vs controllability | The same rank test for continuous-time LTI systems; other books mostly say "controllability". [Stated §7.6] |
| Pole placement vs eigenvalue assignment | The same thing. The book prefers "eigenvalue assignment" because the closed loop is a state space model. |
| *k_f* (feedforward / reference gain) | Not the same as the feedforward of Ch. 1 (acting on a measured disturbance). Here it scales the reference. |
| "Single input" | Theorem 7.3 as stated is for one input. With several inputs, *K* is not unique. |
| *Q_x, Q_u* | The book's notation for the weights. Many texts write *Q* and *R*. |

### Topics the chapter does not explain enough
- **The proof of Theorem 7.1** is outside the book's scope. [Stated §7.1]
- **How to choose eigenvalues for high-order systems**: guidance is limited to "dominant pair plus faster rest". Robust pole placement is in §14.5.
- **Why LQR works** (Riccati derivation): stated without proof.
- **Actuator saturation** is mentioned as a trade-off. Integrator windup, which matters for integral action, is treated only in Ch. 11 §11.4.
- **The big assumption: the full state is measured.** In practice it rarely is. Ch. 8 fixes this.

### Note for Drillbotics [Inferred]
For WOB/RPM control, a PID loop (Ch. 11) is likely enough. State feedback with integral action (Module D) is the systematic way to handle **coupled** WOB and RPM dynamics (a multi-input system), and LQR (Module E) is the usual tool for it. If you try the **inverted pendulum** mentioned in the team plan, Ch. 7 is the standard design route: reachability check, then LQR or pole placement.
