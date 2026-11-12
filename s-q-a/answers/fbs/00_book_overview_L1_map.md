# Feedback Systems (Åström & Murray, 2nd ed.): Book-Level Layer 1 Map

> Source: `materials/control_system/fbs-public_24Jul2020.pdf` (Prefaces + all 15 chapters).
> This is the **index and cross-chapter map**. Each chapter has its own detailed Layer 1 file (linked below).
>
> **Tags:** **[Stated]** = the book says so (mostly in its Prefaces). **[Inferred]** = my recommendation.

---

## 1. Domain Overview

**What the subject studies:** how to use **feedback** (acting on the difference between desired and actual behavior) to make dynamical systems behave as we want: stable, accurate, fast, robust to uncertainty, and insensitive to disturbances and noise. [Stated, Ch. 1]

**The book's stated goals:** to teach "why feedback is useful", "the language and basic mathematics of control", the key paradigms, how to "solve simple feedback problems using back-of-the-envelope techniques", how to "recognize fundamental limitations", and to give "a feel for available design methods". [Stated, Preface 1st ed.]

**Structure of the book [Stated, Preface 1st ed.]:**
- **First half (Ch. 3–8): state space.** Modeling, dynamic behavior, linear systems, state feedback, output feedback.
- **Second half (Ch. 9–14): "classical control" (frequency domain).** Transfer functions, Nyquist, PID, loop shaping, robustness, limits.
- **Ch. 1–2** introduce feedback; **Ch. 15** puts loops into whole-system architectures.

**Math prerequisites [Stated, Preface 1st ed.]:** linear algebra (matrices, vectors, eigenvalues); ODEs (homogeneous and particular solutions); complex numbers; complex variables for some advanced sections. **Laplace transforms are *not* required.** They appear only in §9.3.

---

## 2. Chapter Files

| Ch. | Title | File | One-line role |
|---|---|---|---|
| 1 | Introduction | [ch01](ch01/ch01_introduction_L1_map.md) | Vocabulary and motivation: feedback vs feedforward, uses, P/I/D preview, layers |
| 2 | Feedback Principles | [ch02](ch02/ch02_feedback_principles_L1_map.md) | A quick tour of the whole book with simple models: disturbances, tracking, robustness, positive feedback |
| 3 | System Modeling | [ch03](ch03/ch03_system_modeling_L1_map.md) | State space models, difference equations, block diagrams, uncertainty |
| 4 | Examples | [ch04](ch04/ch04_examples_L1_map.md) | A reference catalog of running examples (cruise control, bicycle, op amp, …) |
| 5 | Dynamic Behavior | [ch05](ch05/ch05_dynamic_behavior_L1_map.md) | Equilibria, phase portraits, stability, Lyapunov |
| 6 | Linear Systems | [ch06](ch06/ch06_linear_systems_L1_map.md) | Matrix exponential, convolution, step and frequency response, linearization |
| 7 | State Feedback | [ch07](ch07/ch07_state_feedback_L1_map.md) | Reachability, eigenvalue assignment, integral action, LQR |
| 8 | Output Feedback | [ch08](ch08/ch08_output_feedback_L1_map.md) | Observability, observers, Kalman filter, 2DOF architecture |
| 9 | Transfer Functions | [ch09](ch09/ch09_transfer_functions_L1_map.md) | *G(s)*, block algebra, poles and zeros, Bode plot |
| 10 | Frequency Domain Analysis | [ch10](ch10/ch10_frequency_domain_analysis_L1_map.md) | Loop transfer function, Nyquist criterion, stability margins |
| 11 | PID Control | [ch11](ch11/ch11_pid_control_L1_map.md) | PID, tuning, windup, implementation |
| 12 | Frequency Domain Design | [ch12](ch12/ch12_frequency_domain_design_L1_map.md) | Gang of Four, specs, loop shaping, feedforward, root locus |
| 13 | Robust Performance | [ch13](ch13/ch13_robust_performance_L1_map.md) | Uncertainty models, robust stability and performance, H∞ overview |
| 14 | Fundamental Limits | [ch14](ch14/ch14_fundamental_limits_L1_map.md) | RHP poles and zeros, delays, Bode integral, nonlinear limits |
| 15 | Architecture and System Design | [ch15](ch15/ch15_architecture_and_system_design_L1_map.md) | Design V, layered and bottom-up architectures, interaction, adaptation and learning |

---

## 3. Cross-Chapter Dependency Map

```
                         Ch.1 Introduction (vocabulary)
                                   │
                         Ch.2 Feedback Principles  (preview of everything; skippable but recommended)
                                   │
                         Ch.3 System Modeling ──────────► Ch.4 Examples (reference; read on demand)
                                   │
                         Ch.5 Dynamic Behavior (stability)
                                   │
                         Ch.6 Linear Systems (e^{At}, responses, linearization)
                    ┌──────────────┴──────────────────┐
           STATE-SPACE PATH                    FREQUENCY-DOMAIN PATH
                    │                                  │
           Ch.7 State Feedback                Ch.9 Transfer Functions  (uses Ch.6 freq. response;
                    │                                  │                 Ex. 9.10 uses Ch.8)
           Ch.8 Output Feedback               Ch.10 Nyquist & Margins
                    │                                  │
                    │                         Ch.11 PID  (also uses Ch.7 §7.4 integral action, Ch.8 §8.5)
                    │                                  │
                    │                         Ch.12 Loop Shaping, Gang of Four
                    │                                  │
                    └──────────────┬───────────────────┘
                                   ▼
                         Ch.13 Robust Performance
                                   │
                         Ch.14 Fundamental Limits  (uses Ch.7 pole placement + Ch.9–13)
                                   │
                         Ch.15 Architecture & System Design (synthesis + survey)
```

**Must come first:** Ch. 1 → Ch. 3 → Ch. 5 → Ch. 6. Everything after depends on *state*, *stability*, and *linear response*. [Inferred from cross-references]

**Two parallel paths after Ch. 6 [Inferred]:**
- **State space path:** Ch. 7 → 8.
- **Frequency path:** Ch. 9 → 10 → 11 → 12.
- They are mostly independent, but they meet in a few places: Ch. 9 Ex. 9.10 (observer-based controller as a transfer function), Ch. 11 (integral action from §7.4, anti-windup for Ch. 8 controllers), Ch. 14 §14.5 (robust *pole placement* = Ch. 7 + frequency insight).

**Book's suggested course plans [Stated, Preface 1st ed.]:**
- 10-week course: Ch. 1–3, 5–7, 9–12, about one per week.
- 14–15-week course: the whole book, with extra time on modeling (Ch. 2–3) and robust performance (Ch. 13).

**Suggested order for this project's goals (Drillbotics control work) [Inferred]:**
1. Ch. 1, 2, 3 (focus on state space and block diagrams), 4.1 (cruise control)
2. Ch. 5 (stability essentials), 6 (linearization, step response)
3. **Ch. 9 → 10 → 11** (transfer functions, margins, PID), to get to working WOB/RPM loops quickly
4. Ch. 12 (sensitivity, feedforward, nested loops)
5. Ch. 7 → 8 (state feedback, observers, Kalman filter), for estimation and multivariable needs
6. Ch. 14 (limits: actuators, delays, friction), Ch. 15 (architecture for the Phase I report)
7. Ch. 13 (robustness), when moving the controller from sim to hardware

---

## 4. Book-Wide Priority Concepts (the "spine")

| Priority | Concepts (chapter) |
|---|---|
| **Essential** | Feedback vs feedforward (1); state, *dx/dt = f(x,u)*, *A,B,C,D* (3); equilibrium, stability, eigenvalue test (5); linearization, step response specs (6); transfer function, poles and zeros, Bode plot (9); loop transfer function, Nyquist, gain and phase margins (10); PID, windup (11); *S*, *T*, loop shape (12) |
| **Supporting** | Reachability and eigenvalue assignment (7); observability and observers (8); feedforward design (12); robust stability *\|δ\| < 1/\|T\|* (13); RHP zero / delay limits (14); cascade and selector control, design V (15) |
| **Advanced** | Lyapunov (5); Jordan form (6); LQR (7); Kalman filter / LQG (8); general Nyquist, describing functions (10); ν-gap, H∞ (13); Bode integral, maximum modulus (14); MPC, adaptive control, learning (15) |

---

## 5. Book-Wide Gaps and Notes

- **Notation (2nd ed.):** load disturbance = *v*, measurement noise = *w*. Many other texts (e.g., the Franklin–Powell book also in `materials/`) use *d* and *n*. [Stated, Preface 2nd ed.]
- **No Laplace dependence.** If your course uses Laplace transforms, §9.3 is the bridge. [Stated, Preface 1st ed.]
- **De-emphasized topics:** root locus, lead/lag compensation details, and hand-sketching Bode and Nyquist plots. [Stated, Preface 1st ed.] Use software, or a second textbook for these.
- **Probability** (for the Kalman filter, Ch. 8) and **complex variables** (for Ch. 10, 14 proofs) are assumed, not taught.
- **Ch. 15 is a draft-quality survey** in the electronic edition (e.g., an empty citation in §15.8). Use it for orientation, not as a detailed reference.
- **Discrete-time / digital control** is spread thinly (Ch. 3, 6 §6.3, 8 §8.4–8.5, 11 §11.5). There is no dedicated chapter.
