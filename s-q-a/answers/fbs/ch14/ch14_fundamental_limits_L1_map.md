# Chapter 14: Fundamental Limits — Layer 1 Knowledge Map

> Source: Åström & Murray, *Feedback Systems*, 2nd ed., Chapter 14, pp. 14-1 to 14-32.
> Layer: 1 (knowledge map).
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **what no controller can achieve**: the limits on performance and robustness that come from the process itself. [Stated, chapter intro] The sources of limits are:
- **Non-minimum phase dynamics**: time delays, right-half-plane (RHP) zeros, and RHP poles. They impose "severe limits".
- **Large-signal nonlinearities**: limited actuator rate and power, protective constraints.
- **Small-signal nonlinearities**: measurement noise, friction, quantization.

It also draws consequences for loop shaping and gives rules for robust pole placement. [Stated, chapter intro]

### Why it matters
- The epigraph: a rear-steered bike "turned out to be safe in an unexpected way: No one could ride it." Some systems are **hard to control by design**, and no clever controller fixes them. [Stated, epigraph; the bike is analyzed in Ex. 14.10]
- "It is particularly important to recognize fundamental limits … early in the design process." Bad mechanical, sensor, or actuator choices create limits that control cannot remove. [Stated §14.1]

### Position in the book
New in the 2nd edition. The material was gathered from the frequency-domain design and robust performance chapters, with new sections on robust pole placement and nonlinear limits added. [Stated, Preface 2nd ed.] It proves the trade-off quoted at the start of Ch. 12 (Stein) and quantifies the "unmodeled dynamics limit performance" warning of §2.3. [Inferred]

### Major components
1. System design considerations: (strong) stabilizability, RHP zeros, time delays (§14.1)
2. Bode's integral formula: the "waterbed effect" (§14.2) ★
3. Gain crossover frequency inequality: how fast can the loop be? (§14.3) ★
4. The maximum modulus principle: lower bounds on sensitivity peaks (§14.4)
5. Robust pole placement: fast process poles, slow process zeros, design rules (§14.5)
6. Nonlinear effects: actuation limits, measurement noise, friction, quantization (§14.6)

### After this chapter you should be able to:
- Explain why RHP zeros and time delays cap the achievable bandwidth, and why RHP poles force a *minimum* bandwidth.
- State Bode's integral formula and explain the waterbed effect: pushing *\|S\|* down in one band pushes it up elsewhere, and more so for unstable processes.
- Use the gain crossover frequency inequality to estimate rough limits (e.g., ω_gc ≲ 0.6·z for an RHP zero *z*; ω_gc ≳ 1.7·p for an RHP pole *p*, for a phase lag allowance of π/3).
- Explain why a system with an RHP pole close to an RHP zero is nearly impossible to control robustly.
- Apply the robust pole placement rules (do not try to move fast stable poles or slow stable zeros).
- Name the nonlinear effects that limit speed (actuator power) and precision (noise, friction, quantization).

---

## 2. Knowledge Structure

### Module A — System design considerations (§14.1)
**Purpose:** Identify limits that come from the structure of the process. [Stated §14.1]

| Concept | One-line gloss |
|---|---|
| Limits must be recognized early | Mechanical design, sensor placement, and actuator choice decide the limits. [Stated §14.1] |
| Intuitive limits | Time delays prevent fast control; unstable systems require fast control, which needs fast sensors and actuators. [Stated §14.1] |
| Stabilizability | Can *some* controller stabilize the process? [Stated §14.1] |
| Strong stabilizability (**Theorems 14.1, 14.2**) | Can a *stable* controller stabilize it? This holds iff there is an even number of real poles between every pair of real RHP zeros (the parity interlacing condition). [Stated §14.1] |
| RHP zeros and time delays | Cause **inverse response** and phase lag without a gain change. RHP zeros can sometimes be removed by **choosing different sensors or outputs**. [Stated §14.1, Ex. 14.1] |

**Example:** vehicle steering: an RHP zero when reversing; it is removed by measuring at the rear axle. (14.1)

**Prerequisites:** Ch. 9 (poles, zeros, RHP, time delay); Ch. 10 §10.4 (minimum phase); Ch. 7–8 (stabilization, reachability).

---

### Module B — Bode's integral formula (§14.2) ★
**Purpose:** Prove that sensitivity cannot be reduced at all frequencies. [Stated §14.2]

| Concept | One-line gloss |
|---|---|
| Recall: \|*S(iω)*\| < 1 ⇒ disturbances attenuated; > 1 ⇒ amplified | [Stated §14.2, recalling §12.2] |
| **Theorem 14.3** (Bode's integral formula) | *∫₀^∞ log\|S(iω)\| dω = π Σ Re(p_k)* (sum over RHP poles of *L*), under a roll-off condition. [Stated §14.2] |
| Waterbed effect | The *area* of log\|S\| is fixed: lowering it somewhere raises it elsewhere. [Stated §14.2] |
| Unstable processes pay more | RHP poles make the integral *positive*: there is net amplification. [Stated §14.2; matches the Stein epigraph of Ch. 12] |

**Example:** the X-29 aircraft (14.2).

**Prerequisites:** Ch. 12 (sensitivity function *S*); Ch. 9 (RHP poles); integrals; logarithms.

---

### Module C — Gain crossover frequency inequality (§14.3) ★
**Purpose:** Turn non-minimum phase dynamics into concrete limits on loop speed. [Stated §14.3]

| Concept | One-line gloss |
|---|---|
| Factorization *P = P_mp · P_ap* | Minimum phase part × all-pass part. The all-pass part has gain 1 and carries the "extra" phase lag. [Stated §14.3] |
| **Theorem 14.4** (gain crossover frequency inequality) | The phase lag of *P_ap* at ω_gc plus the phase needed by the minimum phase part must stay within the budget set by the required phase margin. [Stated §14.3] |
| RHP zero *z* ⇒ upper limit | Roughly ω_gc < 0.6·z (for an allowed lag of π/3). [Stated §14.3, Ex. 14.3] |
| RHP pole *p* ⇒ lower limit | Roughly ω_gc > 1.7·p. [Stated §14.3, Ex. 14.4] |
| RHP pole–zero pair | Requires *z* ≫ *p*. If they are close, robust control is impossible. [Stated §14.3, Ex. 14.5] |
| RHP pole + time delay | The delay caps ω_gc while the pole requires a high ω_gc, so the pole must be slow compared to the delay. [Stated §14.3, Ex. 14.6] |
| Time delay τ ⇒ upper limit | ω_gc is limited to roughly ~1/τ. [Inferred: follows from the delay's phase −ωτ in the same inequality; the book treats delays alongside RHP zeros] |

**Examples:** 14.3–14.7 (balance system with both RHP poles and zeros).

**Prerequisites:** Ch. 10 (phase margin, Bode's gain–phase relation, minimum phase); Ch. 12 (loop shaping, crossover).

---

### Module D — The maximum modulus principle (§14.4)
**Purpose:** Simple *lower bounds* on the sensitivity peaks. [Stated §14.4]

| Concept | One-line gloss |
|---|---|
| **Theorem 14.5** (maximum modulus principle) | An analytic function's maximum magnitude on a region is reached on the boundary. [Stated §14.4] |
| Interpolation constraints | At an RHP zero *z*: *S(z) = 1*, *T(z) = 0*. At an RHP pole *p*: *S(p) = 0*, *T(p) = 1*. [Inferred: the standard constraints used in Ex. 14.8–14.9] |
| Sensitivity lower bounds | *M_s* (or *M_t*) must exceed a bound set by the RHP pole and zero locations; it is large when *p ≈ z*. [Stated §14.4, Ex. 14.9] |
| Rear-wheel-steered bicycle | It has an RHP pole and zero close together, so it is practically unrideable. This explains the epigraph. [Stated §14.4, Ex. 14.10] |

**Prerequisites:** Module C; Ch. 12–13 (*M_s*, *M_t*); complex analysis (analytic functions).

---

### Module E — Robust pole placement (§14.5)
**Purpose:** Pole placement (Ch. 7) does not check robustness. Use pole and zero insight to choose closed loop poles that give robust designs. [Stated §14.5]

| Concept | One-line gloss |
|---|---|
| Fast stable process poles | Poles faster than the intended bandwidth. **Do not try to move them**: it costs large gains and poor robustness. [Stated §14.5, Ex. 14.11] |
| Slow stable process zeros | Zeros slower than the intended bandwidth. They cause overshoot or poor robustness if ignored. Place closed loop poles near them, or cancel them carefully. [Stated §14.5, Ex. 14.12] |
| Design rules for robust pole placement | A list of rules based on *M_t* expressions: choose the bandwidth relative to the process poles and zeros, and avoid moving poles that need not move. [Stated §14.5] |

**Examples:** PI for a first-order system (14.11), vehicle steering (14.12).

**Prerequisites:** Ch. 7 (eigenvalue assignment); Ch. 11–12 (PI, *M_t*); Ch. 13 (robustness measures).

---

### Module F — Nonlinear effects (§14.6)
**Purpose:** Limits from large-signal and small-signal nonlinearities. [Stated §14.6]

| Concept | One-line gloss |
|---|---|
| Actuation limits | Limited power and rate set a bound on response speed (e.g., current limits in servo motors). [Stated §14.6, Ex. 14.13] |
| Measurement noise | The controller feeds sensor noise into the process. This limits how high the gain can go and therefore the achievable precision. [Stated §14.6] |
| Friction | Causes limit cycles or poor precision in positioning (cart–pendulum experiments). [Stated §14.6, Ex. 14.14] |
| Quantization (A/D, D/A, numerics) | Finite resolution bounds the precision. [Stated §14.6] |

**Prerequisites:** Ch. 11 §11.4 (saturation, windup); Ch. 5 (limit cycles); Ch. 10 §10.5 (describing functions, for friction-induced oscillation).

---

## 3. Dependency Map

```
Ch. 9 (RHP poles/zeros, delay)   Ch. 10 (phase margin, min. phase, Bode gain–phase)
Ch. 12 (S, T, loop shaping, crossover)   Ch. 13 (M_s, M_t robustness)
              └──────────────────────┬──────────────────────┘
                                     ▼
Module A: DESIGN CONSIDERATIONS (stabilizability, strong stabilizability, RHP zeros & delays)
                                     │
          ┌──────────────────────────┼──────────────────────────┐
          ▼                          ▼                          ▼
Module B: BODE INTEGRAL     Module C: CROSSOVER INEQUALITY   Module F: NONLINEAR LIMITS
(waterbed; needs S)         P = P_mp·P_ap → limits from z,   (actuators, noise, friction,
                            p, z/p pairs, delays             quantization; needs Ch. 11, 5)
                                     │
                                     ▼
                            Module D: MAXIMUM MODULUS
                            (lower bounds on M_s/M_t; rear-steer bike)
                                     │
                                     ▼
                            Module E: ROBUST POLE PLACEMENT (needs Ch. 7 + C + D insight)
```

**Must be understood first:** RHP zeros, RHP poles, and delays as non-minimum phase elements → the gain crossover frequency inequality (what limits ω_gc from above and below).

**Can be learned independently:**
- Module B (Bode integral) needs only *S* and RHP poles. It does not depend on C.
- Module F (nonlinear effects) does not depend on B–E.
- Strong stabilizability (in A) is a side topic; nothing later needs it.

**Depend on several earlier concepts:**
- The crossover inequality (C) needs the phase margin (Ch. 10), the all-pass factorization (C), **and** Bode's gain–phase relation (Ch. 10 §10.4).
- The maximum modulus bounds (D) need *S*/*T* (Ch. 12), RHP poles and zeros (Ch. 9), **and** analytic function theory.
- Robust pole placement (E) needs eigenvalue assignment (Ch. 7), *M_t* (Ch. 12–13), **and** the pole/zero limit insights (C, D).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Limits come from the process, not the controller; RHP zero ⇒ upper limit on bandwidth; RHP pole ⇒ lower limit on bandwidth; time delay ⇒ upper limit; Bode's integral / waterbed effect (qualitative); actuator limits bound speed; noise bounds precision |
| **Supporting** | Gain crossover frequency inequality with its rules of thumb (0.6·z, 1.7·p); RHP pole–zero pair difficulty; removing RHP zeros by changing the sensor/output; robust pole placement rules (leave fast poles and slow zeros alone); friction and quantization effects |
| **Advanced / postpone** | Strong stabilizability (Theorems 14.1–14.2); exact form of Bode's integral and its conditions; maximum modulus principle and interpolation bounds; X-29 example; detailed *M_t* expressions in pole placement |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Ch. 9, 10, 12, 13.** This chapter uses all of them.
- **Complex analysis** (for §14.2 and §14.4): analytic functions, Cauchy's integral theorem (behind Bode's integral), the maximum modulus principle. The theorems can be *used* without the proofs.
- **Logarithms and integrals** (Bode's integral is an integral of log\|S\|).

### Terms that need clarification
| Term | Issue |
|---|---|
| Non-minimum phase | Bode's term for systems with more phase lag than their gain curve implies (RHP zeros, delays; the book also discusses RHP poles here). [Stated §14.7] |
| All-pass transfer function | Gain 1 at all frequencies; it only adds phase (e.g., *(z − s)/(z + s)*, *e^{−sτ}*). |
| Waterbed effect | A metaphor for Bode's integral: pushing down in one place raises another. |
| Strong stabilizability | Stabilizable by a controller that is *itself* stable. |
| "Fast" pole / "slow" zero | Relative to the *intended closed loop bandwidth*, not an absolute number. [Stated §14.5] |
| Inverse response | The step response first goes the "wrong way". This is the time-domain signature of an RHP zero. |

### Topics the chapter does not explain enough
- **Proofs** of Bode's integral and the crossover inequality rely on complex analysis. They are partially given and are advanced.
- **The rules of thumb** (0.6·z, 1.7·p) depend on an assumed phase margin and lag budget. Know the assumption (π/3 here) before quoting them.
- **Multivariable limits** (MIMO RHP zeros and directions) are not covered. See the §14.7 references.
- **Nonlinear limits** are shown through examples, not general theory.
- **Sensor and actuator selection** as a design activity is mentioned but not systematized. Ch. 15 continues the system-design view.

### Note for Drillbotics [Inferred]
- **Actuator and sensor limits (Module F)** set realistic targets: the lab rig's motor torque and feed-rate limits bound how fast WOB/RPM can respond. Do not spec a bandwidth the hardware cannot deliver.
- **Time delays** (sensor filtering, comms, sample-and-hold) put an upper bound on loop speed (Module C).
- **Friction (Module F)** is directly relevant: the stick-slip in the team plan *is* a friction-induced limit cycle, so Ex. 14.14 is worth reading.
- **Module A's message, "fix limits by design, early"**, supports the team plan's choice to agree on the signal and actuator contract first: sensor placement and actuator choice decide what control can achieve.
