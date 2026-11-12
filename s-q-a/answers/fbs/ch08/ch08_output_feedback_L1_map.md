# Chapter 8: Output Feedback — Layer 1 Knowledge Map

> Source: Åström & Murray, *Feedback Systems*, 2nd ed., Chapter 8, pp. 8-1 to 8-32.
> Layer: 1 (knowledge map).
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **how to control a system when only the output *y* is measured, not the full state *x***. The answer: build an **observer** that *estimates* the state from *u* and *y*, then feed back the estimate. Adding feedforward then gives a general **two-degree-of-freedom** controller architecture. [Stated, chapter intro]

### What problems does it address?
- Can the state be recovered from the measurements? (§8.1: **observability**)
- How do we build a state estimator (observer), and how fast should it be? (§8.2)
- If we feed back the *estimated* state, is the closed loop still what we designed? (§8.3: eigenvalue assignment by output feedback; the **separation** of the design)
- What is the *best* estimator when measurements are noisy? (§8.4: **Kalman filter**, LQG)
- How do real controllers combine feedforward, trajectory generation, feedback, gain scheduling, and computer implementation? (§8.5)

### Position in the book
Chapter 7 assumed all states are measured, which is "highly unrealistic" in many situations. [Stated §8.1] Chapter 8 removes that assumption. Chapters 7 + 8 together complete the **state space design method**, which is the first half of the book. [Stated, Preface 1st ed.]

### Major components
1. Observability (§8.1) ★
2. State estimation: the observer (§8.2) ★
3. Control using the estimated state (§8.3) ★
4. Kalman filtering and LQG (§8.4)
5. State space controller design: 2DOF architecture, trajectory generation, disturbance models, gain scheduling, computer implementation (§8.5)

### After this chapter you should be able to:
- Define observability and test it with *W_o = [C; CA; …; CAⁿ⁻¹]*.
- Design an observer *dx̂/dt = Ax̂ + Bu + L(y − Cx̂)* by placing the eigenvalues of *A − LC*.
- Explain why the closed loop eigenvalues of "state feedback + observer" are the eigenvalues of *A − BK* together with those of *A − LC*.
- Explain in words what the Kalman filter optimizes, and the roles of the process noise and measurement noise covariances.
- Draw the 2DOF architecture (trajectory generator + feedforward + state feedback + observer) and say what each block does.

---

## 2. Knowledge Structure

### Module A — Observability (§8.1) ★
**Purpose:** Find out whether the measurements contain enough information to reconstruct the state. [Stated §8.1]

| # | Concept | One-line gloss |
|---|---|---|
| 1 | **Definition 8.1** (observability) | The state *x(T)* can be determined from *y* and *u* on *[0, T]*. [Stated §8.1] |
| 2 | Observability matrix *W_o = [C; CA; …; CAⁿ⁻¹]* | Built from *A* and *C*. [Stated §8.1] |
| 3 | **Theorem 8.1** (rank condition) | Observable ⇔ *W_o* has full row rank. [Stated §8.1] |
| 4 | Unobservable systems | Some states never affect the output (e.g., the wrong compartment measured). [Stated §8.1, Ex. 8.1] |
| 5 | Observable canonical form | The coordinate form dual to the reachable canonical form. [Stated §8.1] |
| 6 | **Duality** | Observability of *(A, C)* ⇔ reachability of *(Aᵀ, Cᵀ)*. Results transfer by transposition. [Stated §8.1–8.2] |

**Prerequisites:** Ch. 7 Module A (reachability: same structure); Ch. 6 (output response, coordinate changes); linear algebra (rank, transpose).

---

### Module B — State estimation: the observer (§8.2) ★
**Purpose:** Build a dynamical system that outputs an estimate *x̂* of the state. [Stated §8.2]

| # | Concept | One-line gloss |
|---|---|---|
| 1 | Observer structure *dx̂/dt = Ax̂ + Bu + L(y − Cx̂)* | A copy of the model plus a correction proportional to the output prediction error. [Stated §8.2] |
| 2 | Estimation error *x̃ = x − x̂* | Obeys *dx̃/dt = (A − LC)x̃*. [Stated §8.2] |
| 3 | Observer gain *L* | Chosen so that *A − LC* is stable (and fast enough). [Stated §8.2] |
| 4 | **Theorem 8.2** (observer by eigenvalue assignment) | Observable ⇒ the eigenvalues of *A − LC* can be placed arbitrarily. [Stated §8.2] |
| 5 | Computing *L* | Match polynomial coefficients by hand (low order), or use duality with `place`. [Stated §8.2] |
| 6 | Speed trade-off | A faster observer converges quickly but amplifies measurement noise. [Inferred here; the noise issue is formalized in §8.4] |

**Examples:** compartment model (8.2), vehicle steering (8.3).

**Prerequisites:** Module A; Ch. 7 Module B (the same eigenvalue-assignment technique, now applied by duality).

---

### Module C — Control using the estimated state (§8.3) ★
**Purpose:** Combine the observer with state feedback: *u = −Kx̂ + k_f r*. [Stated §8.3]

| Concept | One-line gloss |
|---|---|
| Output feedback controller | Observer + state feedback on *x̂*. The controller is itself a dynamical system of order *n*. [Stated §8.3] |
| **Theorem 8.3** (eigenvalue assignment by output feedback) | If the system is reachable **and** observable, the closed loop characteristic polynomial is *det(sI − A + BK) · det(sI − A + LC)*: both sets can be chosen independently. [Stated §8.3] |
| Separation of design | Design *K* (Ch. 7) and *L* (§8.2) separately. [Stated §8.3; the stochastic "separation theorem" is discussed in §8.6] |
| Assumption *D = 0* | A direct term together with proportional action would create an algebraic loop. [Stated §8.3] |
| Closed loop in *(x, x̃)* coordinates | Block-triangular form, so the eigenvalues separate. [Stated §8.3, Ex. 8.5] |

**Example:** vehicle steering (8.4).

**Prerequisites:** Module B; Ch. 7 (state feedback *K*, *k_f*).

---

### Module D — Kalman filtering and LQG (§8.4)
**Purpose:** Design the *optimal* observer when the process and the measurements are noisy. [Stated §8.4]

| Concept | One-line gloss |
|---|---|
| Stochastic system *x[k+1] = Ax[k] + Bu[k] + v[k], y[k] = Cx[k] + w[k]* | Process noise *v* and measurement noise *w*, both Gaussian white noise. [Stated §8.4] |
| Noise covariances *R_v, R_w* | The "sizes" of the process and measurement noise. [Stated §8.4] |
| Error covariance *P* | The expected squared estimation error. [Stated §8.4] |
| **Theorem 8.4** (Kalman, discrete time) | The gain *L* that minimizes the mean square error comes from a recursive covariance update. [Stated §8.4] |
| **Theorem 8.5** (Kalman–Bucy, continuous time) | *L = PCᵀR_w⁻¹*, where *P* solves a Riccati equation. [Stated §8.4] |
| Disturbance modeling inside the filter | Model a noisy sinusoidal disturbance as extra states (Ex. 8.6). [Stated §8.4] |
| LQG control | LQR (§7.5) + Kalman filter when *u* depends only on the output. [Stated §8.4] |
| Duality with LQR | Kalman filter ↔ LQR: both lead to Riccati equations. [Stated §8.4 / §8.6: dual relationship] |

**Example:** vectored thrust aircraft (8.7).

**Prerequisites:** Modules B and C; Ch. 7 §7.5 (LQR); **probability: random variables, mean, covariance, Gaussian distribution, white noise**. [Stated §8.4: "assumes basic knowledge of random variables and stochastic processes"]

---

### Module E — State space controller design (§8.5)
**Purpose:** Combine everything into the general architecture "at the heart of most modern control systems". [Stated §8.5]

| Concept | One-line gloss |
|---|---|
| Two degree-of-freedom (2DOF) architecture | Trajectory generator (feedforward *u_ff*, desired state *x_d*) + state feedback on the error + observer. [Stated §8.5, Fig. 8.11] |
| Trajectory generation | Compute *(x_d, u_ff)* that satisfy the model and give the desired output. Differential flatness is one method. [Stated §8.5] |
| Disturbance modeling and state augmentation | Model disturbances (constant, ramp, sinusoid) as extra states and estimate them. A constant disturbance model gives **integral action** (Ex. 8.9). [Stated §8.5] |
| Feedback design on the error system | Linearize around the trajectory: *e = x − x_d*, *u_fb = u − u_ff*. [Stated §8.5] |
| Gain scheduling | Change the controller gains with the operating condition (e.g., vehicle speed). [Stated §8.5, Ex. 8.10] |
| Nonlinear estimation | For wide operating ranges, use a nonlinear model in the observer (e.g., the extended Kalman filter). [Stated §8.5] |
| Computer implementation | Sampling, A/D and D/A conversion, discretizing the observer and controller. [Stated §8.5] |

**Example:** vehicle steering lane change (8.8, 8.10).

**Prerequisites:** Modules B–C; Ch. 7 §7.4 (integral action); Ch. 6 §6.4 (linearization), §6.3 (sampling).

---

## 3. Dependency Map

```
Ch. 7: reachability, W_r, eigenvalue assignment (A − BK), LQR
          │ (duality: transpose)
          ▼
Module A: OBSERVABILITY (Def 8.1) → W_o rank test (Thm 8.1) → observable canonical form
          │
          ▼
Module B: OBSERVER  dx̂/dt = Ax̂ + Bu + L(y − Cx̂) → error dynamics (A − LC) → Thm 8.2
          │
          ▼
Module C: OUTPUT FEEDBACK  u = −Kx̂ + k_f r → Thm 8.3: eig(A−BK) ∪ eig(A−LC)  (separation)
          │
          ├────────────────────────────────┐
          ▼                                ▼
Module D: Kalman filter (+ probability)   Module E: 2DOF architecture
          → LQG (+ LQR from Ch. 7)          trajectory generation, disturbance models
                                            (→ integral action), gain scheduling,
                                            nonlinear estimation (needs D), computer impl.
```

**Must be understood first:** observability → observer error dynamics *A − LC* → Theorem 8.3 (separation of the eigenvalues).

**Can be learned independently:**
- Module D (Kalman) and Module E (architecture) are mostly independent. Only "nonlinear estimation" in E builds on D. [Inferred]
- Within E, *gain scheduling* and *computer implementation* can each be read alone.

**Depend on several earlier concepts:**
- Theorem 8.3 needs reachability (Ch. 7) **and** observability (A) **and** both eigenvalue assignment results.
- LQG needs the Kalman filter (D) **and** LQR (Ch. 7 §7.5).
- Integral action via a disturbance model (E) needs the observer (B), state augmentation (Ch. 7 §7.4), **and** the disturbance modeling idea.
- Feedback design around a trajectory (E) needs linearization (Ch. 6 §6.4) **and** state feedback (Ch. 7).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Observability (definition and rank test); duality with reachability; observer structure; error dynamics *A − LC*; Theorem 8.3 (separation of controller and observer eigenvalues); 2DOF idea (feedforward + feedback) |
| **Supporting** | Observable canonical form; computing *L*; observer speed vs noise trade-off; Kalman filter intuition (balancing model trust against measurement trust); disturbance modeling → integral action; gain scheduling |
| **Advanced / postpone** | Kalman filter derivations (Theorems 8.4, 8.5) and Riccati equations; LQG; continuous-time stochastic processes; differential flatness; nonlinear estimation / EKF; computer implementation details |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Ch. 7** in full. This chapter is its mirror image.
- **Linear algebra:** rank, transpose, block-triangular matrices (their eigenvalues are those of the diagonal blocks).
- **Probability and statistics** (for §8.4): random variables, expectation, covariance matrices, Gaussian distributions, white noise. The book assumes this and does not teach it. [Stated §8.4]

### Terms that need clarification
| Term | Issue |
|---|---|
| Observer vs estimator vs filter | The book uses them almost interchangeably. "Kalman filter" is an optimal observer for noisy systems. |
| *x̂* vs *x̃* | Estimate vs estimation error (*x̃ = x − x̂*). |
| Separation principle / theorem | Deterministic version: the eigenvalues separate (Thm 8.3). Stochastic version: the optimal LQG controller = LQR + Kalman filter. The book says the latter is "mathematically subtle". [Stated §8.6] |
| Noise symbols *v*, *w* | 2nd edition: *v* = process disturbance, *w* = measurement noise. [Stated, Preface 2nd ed.] |
| Feedforward | Here *u_ff* comes from a trajectory generator. Related to Ch. 1's feedforward, but generated from the model and the reference. |
| 2DOF | The same concept as §2.4, now in state space form. It is extended in §12.4. |

### Topics the chapter does not explain enough
- **The stochastic theory** behind the Kalman filter: "beyond the scope of this text". [Stated §8.4]
- **Robustness of observer-based controllers.** The separation result says nothing about robustness: LQG can have poor stability margins. That issue is in Ch. 13–14, not here. [Inferred]
- **The extended Kalman filter and other nonlinear estimators**: only mentioned.
- **Differential flatness / trajectory generation**: sketched with one example.
- **Computer implementation** (sampling, quantization, timing) is brief. More appears in §11.5.

### Note for Drillbotics [Inferred]
- **Survey qualification and magnetometer noise** (team plan) are estimation problems. The Kalman filter (Module D) is the standard tool for fusing noisy inclination and azimuth sensors with a motion model.
- The team plan's **"trajectory plan + tracking control"** is exactly Module E's 2DOF architecture: the trajectory generator gives the plan, and the feedback corrects deviations.
- Downhole states (bit torque, bit RPM) are usually **not measured directly** at the surface. Observability (Module A) tells you whether surface measurements can reconstruct them.
