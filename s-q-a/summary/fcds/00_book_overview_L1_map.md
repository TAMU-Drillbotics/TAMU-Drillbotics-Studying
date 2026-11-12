# Feedback Control of Dynamic Systems (Franklin, Powell & Emami-Naeini, 7th ed.): Book-Level Layer 1 Map

> Source: `materials/control_system/Feedback Control of Dynamic Systems.pdf` (Preface + Chapters 1–10 + Appendix A).
> This is the **index and cross-chapter map**. Each chapter has its own detailed Layer 1 file (linked below).
>
> **Tags:** **[Stated]** = the book says so (mostly in its Preface, chapter "Perspective"/"Overview" pages, and chapter Summaries). **[Inferred]** = my recommendation or reading; verify it.
>
> **Short name used in these files:** FPE = this book (Franklin–Powell–Emami-Naeini). FBS = *Feedback Systems* (Åström & Murray), the other textbook in `materials/`, mapped in `s-q-a/summary/fbs/`.

---

## 1. Domain Overview

**What the subject studies:** how to make the output of a *dynamic system* (a system whose present behavior depends on its past, described by differential equations) follow a desired value, by **measuring the output and feeding it back** to a controller that adjusts the input. [Stated, Ch. 1 Perspective]

**What problems it addresses [Stated, §1.3]:** a well-designed feedback system must be
1. **stable** (an absolute requirement),
2. able to **track** a reference command,
3. able to **reject disturbances**, and
4. **robust**: still meet 1–3 when the model used for design is wrong or the plant changes over time.

**The book's approach [Stated, Preface]:** it "combine[s] analysis with design using the three approaches of the **root locus**, **frequency response**, and **state-variable equations**." The authors want you to do basic calculations by hand and make rough sketches of root loci and Bode plots **as a sanity check on computer (Matlab) results**.

**Structure of the book [Stated, Preface and §1.5]:**
- **Foundations (Ch. 1–3):** what feedback is; how to write physical models; how to compute and interpret dynamic response (Laplace transforms, poles and zeros, stability).
- **Feedback analysis (Ch. 4):** the basic equations of feedback, steady-state error, PID.
- **The three design methods (Ch. 5, 6, 7):** root locus, frequency response, state space. "These are alternative means to the same end." [Stated, §1.5]
- **Implementation and reality (Ch. 8, 9):** digital (computer) implementation; nonlinear effects.
- **Synthesis (Ch. 10):** a step-by-step design procedure applied to real case studies.

**After the whole book you should be able to [Inferred from chapter objectives]:**
- Write a linear model of a mechanical, electrical, or electromechanical plant and get its transfer function.
- Predict a step response from pole and zero locations, and test stability.
- Explain what feedback buys you (and what it costs), and compute steady-state errors.
- Design a controller (PID, lead/lag, or state feedback with an estimator) using any of the three methods.
- Convert a continuous controller into code that runs on a computer at a chosen sample rate.
- Recognize nonlinear effects (saturation, windup, limit cycles) and know which tool to reach for.

**Prerequisites [Stated, Preface]:** "dynamics or circuit analysis and Laplace transforms" for the course sequence; "an elementary understanding of matrix algebra" for the state-space material. **Unlike FBS, this book uses Laplace transforms everywhere.** Ch. 2 and Ch. 3 are written as review for students who lack some of this.

---

## 2. Chapter Files

| Ch. | Title (pages) | File | One-line role |
|---|---|---|---|
| 1 | An Overview and Brief History of Feedback Control (1–22) | [ch01](ch01/ch01_overview_and_history_L1_map.md) | Vocabulary: components of a feedback loop, open vs closed loop, the four goals, the "gain dilemma" |
| 2 | Dynamic Models (23–83) | [ch02](ch02/ch02_dynamic_models_L1_map.md) | Writing differential equations from physics: mechanical, electrical, electromechanical, thermal, fluid |
| 3 | Dynamic Response (84–179) | [ch03](ch03/ch03_dynamic_response_L1_map.md) | Laplace transform, transfer function, block diagrams, poles → response, time specs, stability, Routh |
| 4 | A First Analysis of Feedback (180–233) | [ch04](ch04/ch04_first_analysis_of_feedback_L1_map.md) | *S* and *T*, tracking/regulation/sensitivity, system type, PID, Ziegler–Nichols, feedforward |
| 5 | The Root-Locus Design Method (234–307) | [ch05](ch05/ch05_root_locus_L1_map.md) | How closed-loop poles move as a gain changes; lead, lag, notch compensation |
| 6 | The Frequency-Response Design Method (308–432) | [ch06](ch06/ch06_frequency_response_L1_map.md) | Bode plots, Nyquist criterion, gain and phase margins, loop-shaping compensation, time delay |
| 7 | State-Space Design (433–589) | [ch07](ch07/ch07_state_space_design_L1_map.md) | State equations, controllability, pole placement, LQR/SRL, estimators, integral control |
| 8 | Digital Control (590–636) | [ch08](ch08/ch08_digital_control_L1_map.md) | Sampling, z-transform, discrete equivalents (Tustin, ZOH), sample-rate choice |
| 9 | Nonlinear Systems (637–702) | [ch09](ch09/ch09_nonlinear_systems_L1_map.md) | Linearization, saturation and antiwindup, describing functions, phase plane, Lyapunov |
| 10 | Control System Design: Principles and Case Studies (703–803) | [ch10](ch10/ch10_design_principles_and_case_studies_L1_map.md) | A 9-step design outline and six case studies |
| App. A | Laplace Transforms (804–818) | (covered in the Ch. 3 file) | Transform tables, properties, partial fractions, initial and final value theorems |

---

## 3. Cross-Chapter Dependency Map

```
                    Ch.1 Overview (vocabulary, goals)
                               │
                    Ch.2 Dynamic Models  (ODEs from physics)
                               │
                    Ch.3 Dynamic Response ★ (Laplace, G(s), poles/zeros, specs, stability)
                               │
                    Ch.4 First Analysis of Feedback ★ (S, T, system type, PID)
                               │
          ┌────────────────────┼─────────────────────┐
          ▼                    ▼                     ▼
   Ch.5 Root Locus      Ch.6 Frequency Resp.   Ch.7 State-Space
   (s-plane)            (Bode, Nyquist,        (A,B,C,D, K, L)
          │              margins)                    │
          │  lead/lag introduced in 5.4,             │  uses root locus (SRL, §7.6.2)
          │  redesigned in 6.7                       │  and margins (LQR robustness, §7.11)
          └────────────────────┬─────────────────────┘
                               ▼
          ┌────────────────────┴─────────────────────┐
          ▼                                          ▼
   Ch.8 Digital Control                       Ch.9 Nonlinear Systems
   (needs a continuous Dc(s) from 4–7;        (uses root locus §9.3, Nyquist §9.4,
    z-plane mirrors s-plane)                   state form §9.5)
          └────────────────────┬─────────────────────┘
                               ▼
                    Ch.10 Design Principles & Case Studies (uses everything)
```

**Must come first:** Ch. 3 and Ch. 4. Every design chapter assumes transfer functions, pole locations, block-diagram algebra, the characteristic equation, and the sensitivity function. [Inferred from cross-references; the Preface calls Ch. 4–7 "the core topics" that need Ch. 2–3]

**The three design chapters (5, 6, 7) are partly parallel [Stated, §1.5: "alternative means to the same end... fundamentally complementary"]:**
- Ch. 5 and Ch. 6 can be read in either order, but Ch. 6 refers back to root-locus ideas (neutral stability in §6.2, lead/lag first met in §5.4). [Inferred from cross-references]
- Ch. 7 depends on Ch. 5 for the symmetric root locus (§7.6.2) and on Ch. 6 for robustness ideas (§7.11). Its core (§7.2–7.7) needs only Ch. 3 plus linear algebra. [Inferred]

**Book's suggested course plans [Stated, Preface]:**
- Ten-week quarter: review Ch. 3, cover Ch. 1, 4, 5, 6; omit most optional sections.
- Second quarter: Ch. 7 and 9 (or parts of Ch. 8).
- Semester: Ch. 1–7, then digital control (Ch. 8), selected nonlinear topics (Ch. 9), and case studies (Ch. 10) if time remains.
- Three quarters: modeling and response (Ch. 2–3), classical control (Ch. 4–6), modern control (Ch. 7–10).

**Suggested order for this project's goals (Drillbotics control work) [Inferred]:**
1. Ch. 1, then Ch. 2 §2.1 (mechanical) and §2.3 (DC motor, gears) — the drill's motors are electromechanical.
2. Ch. 3 in full (this is the most important foundation chapter).
3. Ch. 4 in full (sensitivity, system type, PID): enough to build first WOB/RPM loops.
4. Ch. 6 (frequency response, margins) → Ch. 5 (root locus), for tuning and checking robustness.
5. Ch. 8 (digital control): the controller will run on a computer at a fixed sample rate.
6. Ch. 9 §9.2–9.3 (linearization, saturation, antiwindup): actuator limits are mandatory in the plan.
7. Ch. 7 (state space, estimators): when the inverted-pendulum test rig or multi-state estimation is needed.
8. Ch. 10 §10.1 (design outline), for structuring the Phase I design report.

### Pairing with FBS (the other textbook) [Inferred, from chapter titles and both books' contents]

| FPE chapter | Closest FBS chapters | Note |
|---|---|---|
| 1 Overview | FBS 1, 2 | Both introduce feedback vs feedforward and the goals |
| 2 Dynamic Models | FBS 3, 4 | FPE writes ODEs and transfer functions; FBS writes state-space models |
| 3 Dynamic Response | FBS 5, 6, 9 | FPE uses Laplace; FBS avoids it until §9.3 |
| 4 First Analysis of Feedback | FBS 2, 11, 12 | Sensitivity *S*, *T*; PID |
| 5 Root Locus | FBS 12 (root locus section) | FBS de-emphasizes root locus; FPE gives full rules |
| 6 Frequency Response | FBS 9, 10, 12, 13, 14 | FPE §6.7.7–6.7.8 ≈ FBS robustness and limits |
| 7 State-Space Design | FBS 6, 7, 8 | "controllability" (FPE) = "reachability" (FBS); "estimator" (FPE) = "observer" (FBS) |
| 8 Digital Control | (spread across FBS 3, 6, 8, 11) | FPE has a dedicated chapter; FBS does not |
| 9 Nonlinear Systems | FBS 5 (Lyapunov, phase portraits), 10 (describing functions), 11 (windup) | |
| 10 Design & Case Studies | FBS 15 | |

---

## 4. Book-Wide Priority Concepts (the "spine")

| Priority | Concepts (chapter) |
|---|---|
| **Essential** | Plant, sensor, actuator, controller, open vs closed loop (1); equations of motion from free-body diagrams, DC motor model (2); Laplace transform, transfer function, poles and zeros, block-diagram reduction, ζ and ωₙ, step-response specs, stability = poles in LHP, Routh (3); *S* and *T*, characteristic equation 1 + *GD*꜀ = 0, system type and error constants, P/I/D effects (4); root-locus rules 1–3, lead and lag (5); Bode plot, gain and phase margins, crossover frequency, bandwidth (6); state equations, *u* = −*Kx*, estimator (7); sampling, ZOH, Tustin, sample rate (8); saturation and antiwindup (9); design outline (10) |
| **Supporting** | Electrical and fluid/thermal models (2); convolution, impulse response, BIBO stability, effect of extra zeros and poles (3); Ziegler–Nichols tuning, feedforward by model inversion (4); departure angles, notch compensation (5); Nyquist criterion, Bode gain-phase relation, sensitivity-function specifications (6); controllability/observability tests, LQR/SRL, integral control in state space (7); z-transform, aliasing, anti-alias prefilter (8); describing functions, limit cycles (9); case studies (10) |
| **Advanced** | Distributed-parameter systems (2); Kharitonov theorem (3); negative and two-parameter root loci (5); Bode's sensitivity integral (waterbed), Nichols chart (6); error-space/internal-model design, model following, loop transfer recovery, polynomial (Diophantine) design (7); direct discrete design (8); feedback linearization, Lyapunov's second method, circle criterion (9) |

---

## 5. Book-Wide Gaps and Notes

- **Notation is the opposite of FBS for disturbances and noise.** FPE: plant disturbance = ***W***, sensor noise = ***V***, controller = ***D*꜀(*s*)**, plant = ***G*(*s*)**. FBS (2nd ed.): load disturbance = *v*, measurement noise = *w*, controller *C*(*s*), plant *P*(*s*). Check the letters every time you switch books. [Stated in each book; the comparison is Inferred]
- **State-space notation changed in this edition** from (*F*, *G*, *H*, *J*) to (*A*, *B*, *C*, *D*), and the controller from *D*(*s*) to *D*꜀(*s*). Older notes, solutions, or papers may use the old letters. [Stated, Preface]
- **Much material is on the book's website (www.FPE7e.com), not in this PDF.** Examples: complex-variables review (Appendix WA), matrix theory review (WB), controllability and observability (WC), Ackermann's formula derivation (WD), system identification (W3.7), Mason's rule (W3.2.3), Routh special cases (W3.6.3), digital control intro (W4.5), solution of state equations / matrix exponential (W7.14), discrete state-space design (W8.7). [Stated, Contents] You may need FBS or another source for these.
- **Optional sections** are marked with a triangle (▲) in the book. [Stated, Preface]
- **Scope limits [Stated, §1.5]:** mostly single-input single-output (SISO) systems; multivariable control appears only in the RTP case study (Ch. 10); optimal control is only introduced (Ch. 7); experimental testing and modeling of real hardware is "beyond the scope."
- **PDF quality [Inferred, from text extraction]:** this PDF is a scanned book with OCR. Equations come out garbled when copied as text. Always check formulas against the page image.
