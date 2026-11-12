# Chapter 7: State-Space Design — Layer 1 Knowledge Map

> Source: Franklin, Powell & Emami-Naeini, *Feedback Control of Dynamic Systems*, 7th ed., Chapter 7, pp. 433–589.
> Layer: 1 (knowledge map). What to learn and in what order, not full explanations.
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies the **third design method**: describing the plant by first-order vector equations ẋ = *Ax* + *Bu*, *y* = *Cx* + *Du*, and designing the compensator directly from them. [Stated, Perspective] The end result is still a compensator *D*꜀(*s*), "equivalent to those *D*꜀(*s*) compensators obtained with the other two methods." [Stated, Perspective]

### What problems does it address?
- A compact, general model form that also handles nonlinear and multi-input multi-output systems, and shows internal variables a transfer function hides. [Stated §7.1]
- Placing **all** closed-loop poles where we want them (not only the ones a single gain can reach).
- Doing this when **not all states are measured**: build an **estimator** of the state.
- Adding a reference input and integral action so the output tracks commands with zero steady-state error.

### Major components
1. Why state space; state description; block diagrams (§7.1–7.3)
2. Analysis of the state equations: canonical forms, transfer function, poles, zeros (§7.4)
3. Control-law design by full-state feedback (§7.5) ★
4. Choosing good pole locations: dominant second-order, symmetric root locus/LQR (§7.6)
5. Estimator design (§7.7) ★
6. Compensator = control law + estimator; reference input with an estimator (§7.8, §7.9)
7. Integral control and robust tracking (§7.10)
8. Advanced (▲): loop transfer recovery, polynomial design, time delay (§7.11–7.13); solution of state equations (§7.14, web)

**The design steps [Stated, Chapter Overview]:**
1. Select closed-loop pole locations and develop the control law (§7.5, §7.6).
2. Design an estimator (§7.7).
3. Combine the control law and the estimator (§7.8).
4. Introduce the reference input (§7.5.2, §7.9).

### After this chapter you should be able to:
- Write a state-space model of a physical system and convert between state space and transfer function.
- Compute poles (eigenvalues of *A*) and zeros from (*A*, *B*, *C*, *D*).
- Test controllability and observability with their matrices.
- Design a state-feedback gain *K* by pole placement (Ackermann or `place`) or by LQR (`lqr`).
- Design an estimator gain *L*, and combine it with *K* into a compensator.
- Add integral control in state space for robust zero steady-state error.

---

## 2. Knowledge Structure

### Module A — State-space description (§7.1–7.3)
**Purpose:** Introduce the model form and why it is useful.

| Concept | One-line gloss |
|---|---|
| State-variable (normal) form | The ODEs organized as first-order equations in a vector state; the solution is a trajectory in state space. [Stated §7.1] |
| Three reasons to use it | (1) general models: nonlinear, time-varying, MIMO; (2) geometry (phase-plane idea in *n* dimensions); (3) connects internal and external description: states often measure stored energy. [Stated §7.1] |
| "State-space vs transform methods" | The book prefers these names over "modern vs classical." [Stated §7.1] |
| (*A*, *B*, *C*, *D*) model | ẋ = *Ax* + *Bu*, *y* = *Cx* + *Du*. (Older editions used *F*, *G*, *H*, *J*.) [Stated §7.2, Preface] |
| Choosing state variables | Positions, velocities, capacitor voltages, inductor currents, temperatures: energy-storage variables. [Stated §7.1] |
| Block diagram / analog-computer view | Each state is the output of an integrator; the diagram can be drawn directly from the equations. [Stated §7.3] |

**Prerequisites:** Ch. 2 (ODE models); linear algebra (matrices, vectors, matrix–vector product).

---

### Module B — Analysis of the state equations (§7.4)
**Purpose:** Connect the state model to everything you know from transforms.

| Concept | One-line gloss |
|---|---|
| Canonical forms | **Control** canonical form, **modal** canonical form (diagonal, one mode per state), **observer** canonical form. [Stated §7.4.1, Summary] |
| Similarity transformation | Change of state variables *x* = *Tz*; gives a different (*A*, *B*, *C*, *D*) for the same input–output behavior. [Stated §7.4.1] |
| Controllability matrix | 𝒞 = [*B* *AB* … *A*ⁿ⁻¹*B*]. Nonsingular ⇔ the system can be put in control canonical form. [Stated §7.4.1] |
| Controllability ↔ no cancellation | A SISO system is controllable iff the input excites all natural frequencies, i.e., no pole–zero cancellation. [Stated, Summary] |
| Transfer function from state space | *G*(*s*) = *C*(*sI* − *A*)⁻¹*B* + *D*. [Stated §7.4.2] |
| Poles | Eigenvalues of *A*: det(*pI* − *A*) = 0. [Stated, Summary] |
| Zeros | Values *z* where det[*zI* − *A*, −*B*; *C*, *D*] = 0. [Stated, Summary] |
| Dynamic response | Computing the time response from the state equations (by hand via Laplace, or Matlab). [Stated §7.4.2] |

**Prerequisites:** Module A; Ch. 3 (transfer function, poles, zeros); linear algebra (determinant, inverse, eigenvalues, rank).

---

### Module C — Full-state feedback control law (§7.5) ★
**Purpose:** Place all closed-loop poles by feeding back every state.

| Concept | One-line gloss |
|---|---|
| Control law *u* = −*Kx* | Assume (for now) all states are measured. [Stated §7.5] |
| Closed-loop poles | Roots of det[*sI* − (*A* − *BK*)] = 0, the control characteristic equation α꜀(*s*). [Stated §7.5.1] |
| Pole placement theorem | For any **controllable** system of order *n*, *K* can place the closed-loop poles at any desired set. [Stated, Summary] |
| Ways to find *K* | Match coefficients; transform to control canonical form; **Ackermann's formula**; Matlab `acker`/`place`. Ackermann has poor numerical accuracy. [Stated §7.5.1] |
| Reference input with full-state feedback | *u* = *N*ᵤ*r* − *K*(*x* − *N*ₓ*r*): gives zero steady-state error to a step, but **not robustly** (depends on exact parameters). [Stated §7.5.2, Summary] |

**Prerequisites:** Module B (controllability; *A* − *BK* eigenvalues); Ch. 3 (pole locations ↔ response).

---

### Module D — Choosing pole locations (§7.6)
**Purpose:** Decide *where* to put the poles.

| Concept | One-line gloss |
|---|---|
| Design trade-off | Good poles balance transient response, robustness, and **control effort** (actuator size). [Stated, Summary] |
| Dominant second-order poles | Pick two poles to meet ζ, ωₙ specs; put the rest far to the left. [Stated §7.6.1] |
| LQR (linear quadratic regulator) | Choose *K* to minimize a cost that weighs output error against control effort. [Stated §7.6.2] |
| Symmetric root locus (SRL) | A root locus vs the weight ρ, whose LHP branches give the LQR poles. [Stated §7.6.2] |
| Bryson's rule | Starting values for the LQR weights *Q*, *R* from the maximum acceptable state and input sizes. [Stated §7.6.2] |
| LQR robustness | LQR loops have guaranteed margins: the Nyquist plot avoids a unit circle around −1. [Stated §7.6.2] |

**Prerequisites:** Module C; Ch. 5 (root locus, for the SRL); Ch. 6 (margins, for LQR robustness).

---

### Module E — Estimator (observer) design (§7.7) ★
**Purpose:** Reconstruct the state from the measured output *y* and the input *u*.

| Concept | One-line gloss |
|---|---|
| Why an estimator | Measuring every state is usually impractical or too expensive. [Stated §7.7, Review Q 7.10] |
| Full-order estimator | x̂̇ = *A*x̂ + *Bu* + *L*(*y* − *C*x̂): a model copy corrected by the output error. [Stated §7.7.1] |
| Estimator error dynamics | Error poles are roots of det[*sI* − (*A* − *LC*)] = 0. [Stated §7.7.1] |
| Observability matrix | 𝒪 = [*C*; *CA*; …; *CA*ⁿ⁻¹]. For any **observable** system, *L* can place the estimator poles anywhere. [Stated §7.7.1, Summary] |
| Duality | Estimator design is mathematically the same problem as control design with (*A*ᵀ, *C*ᵀ) (Table 7.1). [Stated §7.7.1] |
| Reduced-order estimator | Estimate only the unmeasured states; lower order but passes more sensor noise. [Stated §7.7.2] |
| Estimator pole selection | Rule of thumb: **2 to 6 times faster** than the control poles; slower if sensor noise is a major concern. [Stated §7.7.3] |
| Optimal estimation (idea) | The best *L* depends on the ratio of process (disturbance) noise to sensor noise. [Stated §7.7.3] |

**Prerequisites:** Module B (observability); Module C (the dual problem).

**Independent of Module D.** You can design an estimator once you know pole placement (C). [Inferred]

---

### Module F — Combined compensator and reference input (§7.8, §7.9)
**Purpose:** Use *u* = −*K*x̂ and turn the result into a transfer function.

| Concept | One-line gloss |
|---|---|
| Combined poles | The closed-loop poles are the **union** of the control-law poles and the estimator poles. [Stated, Summary] |
| Compensator transfer function | *D*꜀(*s*) = −*K*(*sI* − *A* + *BK* + *LC*)⁻¹*L*. Often turns out to be a lead network. [Stated §7.8, Table 7.3] |
| Reference input with estimator | The reference can be introduced so as to assign *n* zeros; the usual choice cancels the estimator poles so a reference change does not excite estimator error. [Stated §7.9, Summary] |

**Prerequisites:** Module C **and** Module E.

---

### Module G — Integral control and robust tracking (§7.10)
**Purpose:** Make tracking robust to parameter changes and constant disturbances.

| Concept | One-line gloss |
|---|---|
| Integral control in state space | Augment the state with the integral of the error, then design *K* for the augmented system. Robust zero error to steps and constant disturbances. [Stated §7.10.1, Summary] |
| Error-space approach (▲) | Combine plant and reference model into an "error space" and design a control law there. [Stated §7.10.2] |
| Internal model principle (▲) | Robust tracking requires a model of the reference (or disturbance) generator inside the controller. [Stated §7.10.2] |
| Model-following design (▲) | Better tracking, but "suffers from robustness problems." [Stated §7.10.3, Summary] |
| Extended estimator (▲) | Estimate an equivalent disturbance too, for robust tracking and rejection. [Stated §7.10.4] |
| Three ways to add integral control | Review Q 7.15 asks for them (augmented state, error-space/internal model, extended estimator). [Stated, Review Questions; the list is Inferred from §7.10's subsections] |

**Prerequisites:** Module C; Ch. 4 (system type, why integrators remove error).

---

### Module H — Advanced topics (▲)
| Concept | One-line gloss |
|---|---|
| Loop transfer recovery (§7.11) | Adjust the estimator so the estimator-based loop regains the good margins of full-state (LQR) feedback. [Stated §7.11] |
| Direct design with rational transfer functions (§7.12) | Pole placement with polynomials (Diophantine equation); often numerically poor. [Stated §7.12, Summary] |
| Pure time delay (§7.13) | Design as if there were no delay, then implement for the delayed plant; sensitive to delay uncertainty. [Stated §7.13, Summary] |
| Solution of state equations (§7.14) | Web only. [Stated §7.14] |

---

## 3. Dependency Map

```
Linear algebra (matrices, det, inverse, eigenvalues)      Ch.2 models, Ch.3 G(s), poles
                 │                                                │
                 └────────────────────┬───────────────────────────┘
                                      ▼
Module A: why state space; (A,B,C,D); choosing states; integrator block diagram
                                      │
                                      ▼
Module B: canonical forms, similarity transform, controllability matrix,
          G(s)=C(sI−A)⁻¹B+D, poles=eig(A), zeros
                                      │
                      ┌───────────────┴────────────────┐
                      ▼                                ▼
Module C ★: u = −Kx, det[sI−(A−BK)]       Module E ★: estimator x̂, A − LC,
   Ackermann/place; Nu, Nx                    observability matrix, duality,
                      │                       reduced-order, pole selection (2–6×)
          ┌───────────┼──────────────┐                │
          ▼           ▼              ▼                │
Module D:        Module G:      Module F ◄────────────┘
dominant poles,  integral       compensator = K + L (union of poles),
SRL/LQR          control,       Dc(s), reference input with estimator
(needs Ch.5,6)   internal model             │
                                            ▼
                               Module H (▲): LTR, polynomial design, time delay
```

**Must be understood first:** the (*A*, *B*, *C*, *D*) model; eigenvalues of *A* as poles; *u* = −*Kx* and the eigenvalues of *A* − *BK*.

**Can be learned independently:**
- Canonical forms (B) can be skimmed; pole placement (C) only needs controllability and *A* − *BK*.
- Pole selection (D) and estimator design (E) do not depend on each other.
- Integral control (G) does not need the estimator (E) for its basic version.

**Depend on several earlier concepts:**
- The combined compensator (F) needs control law (C) **and** estimator (E).
- SRL/LQR (D) needs pole placement (C) **and** the root locus (Ch. 5); its robustness claim needs Nyquist/margins (Ch. 6).
- Loop transfer recovery (H) needs LQR (D) **and** estimators (E) **and** margins (Ch. 6).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | State-variable form; (*A*, *B*, *C*, *D*); choosing state variables; *G*(*s*) = *C*(*sI* − *A*)⁻¹*B* + *D*; poles = eig(*A*); controllability; *u* = −*Kx*; pole placement; reference input (*N*ₓ, *N*ᵤ); estimator equation and *A* − *LC*; observability; estimator 2–6× faster rule; compensator = control law + estimator |
| **Supporting** | Canonical forms and similarity transforms; zeros from state space; Ackermann's formula; dominant second-order poles; LQR and Bryson's rule; duality; reduced-order estimator; integral control by state augmentation; reference input with an estimator |
| **Advanced / postpone** | Symmetric root locus details; LQR limiting behavior and robustness proof; error-space/internal model principle; model following; extended estimator; loop transfer recovery; polynomial (Diophantine) design; time-delay design; history (§7.15) |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Linear algebra:** matrix multiplication, determinant, inverse, rank/nonsingular, eigenvalues and eigenvectors, transpose. The book's review is in web Appendix WB; a brief treatment is at the start of Ch. 7. [Stated, Preface]
- Ch. 3: transfer functions, poles; Ch. 5: root locus (for SRL); Ch. 6: margins (for LQR robustness).

### Terms that need clarification
| Term | Issue |
|---|---|
| Controllability | FBS calls this **reachability**. Same test matrix 𝒞. |
| Estimator | FBS (and many others) call this an **observer**. |
| *K* vs *L* | *K* = control (feedback) gain; *L* = estimator gain. Not the root-locus *L*(*s*) of Ch. 5. |
| "Union of poles" | Often called the **separation principle**; the book states the result without using that name. [Inferred naming] |
| SRL / LQR | Two views of the same optimal design: SRL is a root-locus picture; LQR is the optimization. |
| *N*ₓ, *N*ᵤ | Steady-state state and input per unit reference; they make the DC gain correct but are not robust. |

### Topics the chapter does not explain enough
- **Solving the state equation** (matrix exponential *e*^{*At*}) is web only (§7.14). FBS Ch. 6 covers it in print.
- **Controllability/observability theory** and **Ackermann's derivation** are web only (Appendices WC, WD).
- **Kalman filter / stochastic estimation** is only hinted at (noise ratio in §7.7.3). FBS Ch. 8 goes further.
- **MIMO design** is outside the book's scope (it mostly uses SISO examples). [Stated §7.1]
- **Model from experiments**: the Summary notes that state-space design needs a good model, which frequency-response design may not. [Stated, Summary]

### Note for Drillbotics [Inferred]
- The **inverted pendulum** test rig (considered in the team plan) is the classic Ch. 7 problem: a 4-state model, pole placement or LQR for *K*, and an estimator for unmeasured velocities.
- An **estimator** can reconstruct quantities that are hard to measure downhole (e.g., bit speed from surface measurements and a drill-string model). It is only as good as the model.
- **Integral control in state space** (§7.10.1) gives the robust zero-error behavior you also get from PID's I term.
