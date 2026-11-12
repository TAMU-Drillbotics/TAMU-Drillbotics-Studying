# Chapter 6: Linear Systems — Layer 1 Knowledge Map

> Source: Åström & Murray, *Feedback Systems*, 2nd ed., Chapter 6, pp. 6-1 to 6-36.
> Layer: 1 (knowledge map).
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It specializes the general models of Ch. 3–5 to **linear, time-invariant (LTI) input/output systems**, *dx/dt = Ax + Bu, y = Cx + Du*, and shows that their behavior can be **computed completely**. [Stated, chapter intro]

### The two central tools [Stated, chapter intro]
1. The **matrix exponential** *e^{At}*, which gives the response to initial conditions.
2. The **convolution equation**, which gives the response to inputs.

### What problems does it address?
- What *exactly* does "linear" mean for a system with inputs and initial conditions? (§6.1)
- How do we solve *dx/dt = Ax* in closed form? (§6.2)
- How does an LTI system respond to steps, impulses, and sinusoids? How do we describe that response with numbers (rise time, overshoot, gain, phase)? (§6.3)
- How do we convert between continuous time and discrete time (sampling)? (§6.3)
- How do we get a linear model from a nonlinear one, so that all these tools apply? (§6.4)

### Why it matters
Almost all design methods in Ch. 7–14 assume a linear model. This chapter is the bridge: **nonlinear model → linearization → LTI model → exact response formulas**. [Inferred; the book says in §6.4 that "it is common practice … to design controllers based on an approximate linear model"]

### Major components
1. Basic definitions: linearity, time invariance (§6.1)
2. The matrix exponential: initial condition response, Jordan form, modes (§6.2) ★
3. Input/output response: convolution, coordinate invariance, step response, frequency response, sampling (§6.3) ★
4. Linearization: Jacobian and feedback linearization (§6.4) ★

### After this chapter you should be able to:
- State the superposition principle for a state space system with an initial condition.
- Compute *e^{At}* for a diagonalizable *A*, and explain what the eigenvalues say about the modes.
- Write the full solution *x(t) = e^{At}x(0) + ∫ e^{A(t−τ)}Bu(τ)dτ*.
- Read a step response (rise time, overshoot, settling time, steady-state value) and a frequency response (gain, phase, bandwidth, resonant peak).
- Turn a continuous-time LTI model into a sampled (discrete-time) model.
- Linearize a nonlinear system around an equilibrium point (Jacobian), and describe what feedback linearization does.

---

## 2. Knowledge Structure

### Module A — Basic definitions (§6.1)
**Purpose:** Define linearity and time invariance *precisely* for systems with inputs.

| Concept | One-line gloss |
|---|---|
| Linear input/output system | The output is linear in (initial state, input) together. This gives superposition. [Stated §6.1] |
| Superposition | The response to *αu₁ + βu₂* is *αy₁ + βy₂* (with matching initial conditions). [Stated §6.1] |
| Homogeneous + particular response | Output = response to the initial condition (no input) + response to the input (zero initial state). [Stated §6.1] |
| Time invariance | Shifting the input in time shifts the output by the same amount. [Stated §6.1] |

**Prerequisites:** Ch. 3 (*A, B, C, D* form; linear, time-invariant); Ch. 5 (solutions of ODEs).

---

### Module B — The matrix exponential (§6.2) ★
**Purpose:** Solve *dx/dt = Ax* in closed form.

| # | Concept | One-line gloss |
|---|---|---|
| 1 | Matrix exponential *e^{At}* | Defined by the power series *I + At + (At)²/2! + …*. [Stated §6.2] |
| 2 | **Proposition 6.1** | The solution of *dx/dt = Ax* is *x(t) = e^{At}x(0)*. [Stated §6.2] |
| 3 | Double integrator; undamped oscillator | The two standard examples (Ex. 6.2, 6.3). [Stated §6.2] |
| 4 | Diagonalization | If *A* has distinct eigenvalues, a coordinate change makes it diagonal, so *e^{At}* is diagonal with entries *e^{λᵢt}*. [Stated §6.2] |
| 5 | Jordan form (**Theorem 6.2**) | Repeated eigenvalues may give Jordan blocks. These produce terms like *t·e^{λt}*. [Stated §6.2] |
| 6 | **Proposition 6.3** | With eigenvalues on the imaginary axis: stable ⇔ those Jordan blocks are trivial (1×1). [Stated §6.2] |
| 7 | Eigenvalues and modes | Each eigenvalue/eigenvector pair is a *mode*: a pattern of motion that evolves independently (e.g., coupled spring–mass vibration patterns). [Stated §6.2] |

**Why together:** They all describe the **initial condition (free) response**. Eigenvalues set the *rates*; eigenvectors set the *shapes*.

**Prerequisites:** Module A; Ch. 5 Theorem 5.1 (this module supplies its proof machinery); **linear algebra: eigenvalues, eigenvectors, diagonalization, change of basis**.

---

### Module C — Input/output response (§6.3) ★
**Purpose:** Add inputs and describe responses in engineering terms.

| # | Concept | One-line gloss |
|---|---|---|
| 1 | **Theorem 6.4** (convolution equation) | *x(t) = e^{At}x(0) + ∫₀ᵗ e^{A(t−τ)}Bu(τ)dτ*. [Stated §6.3] |
| 2 | Impulse response *h(t) = Ce^{At}B + Dδ(t)* | The response to a very short pulse. The output is the convolution of *h* with *u*. [Stated §6.3] |
| 3 | Coordinate invariance | Changing state coordinates (*z = Tx*) changes *A, B, C* but **not** the input/output behavior. [Stated §6.3] |
| 4 | Transient vs steady-state response | The short-term part dies out (if stable); the long-term part remains. [Stated §6.3] |
| 5 | Step response | The response to a unit step input. [Stated §6.3] |
| 6 | Step response specs | **Steady-state value**, **rise time**, **overshoot**, **settling time**. [Stated §6.3] |
| 7 | Zero frequency (DC) gain | Steady-state output / constant input = *−CA⁻¹B + D*. [Stated §6.3] |
| 8 | Frequency response | Steady-state response to sinusoids: **gain** (amplitude ratio) and **phase** (shift), as functions of frequency. [Stated §6.3] |
| 9 | Bandwidth, resonant peak | Frequency range of good transmission / the maximum gain at resonance. [Stated §6.3] |
| 10 | Bode plot (first use) | Gain and phase vs frequency on log scales. Fully treated in §9.6. [Stated §6.3] |
| 11 | Sampling | Exact conversion of an LTI system to *x[k+1] = Φx[k] + Γu[k]*, with *Φ = e^{Ah}*, for sampling period *h*. [Stated §6.3] |

**Examples:** coupled spring–mass (6.6), compartment model (6.7), active band-pass filter (6.8), AFM in contact mode (6.9), IBM Lotus server (6.10).

**Prerequisites:** Module B (matrix exponential); integration; complex numbers (for the frequency response); Ch. 3 difference equations (for sampling).

---

### Module D — Linearization (§6.4) ★
**Purpose:** Turn nonlinear models into LTI models, so that everything above applies locally. [Stated §6.4]

| Concept | One-line gloss |
|---|---|
| Equilibrium point / operating point *(x_e, u_e)* | A state–input pair where *f(x_e, u_e) = 0*. [Stated §6.4] |
| Deviation variables | *z = x − x_e*, *v = u − u_e*, *w = y − y_e*. [Stated §6.4] |
| Jacobian linearization | *A = ∂f/∂x, B = ∂f/∂u, C = ∂h/∂x, D = ∂h/∂u*, evaluated at the equilibrium. [Stated §6.4] |
| Validity of the linear model | Good only near the operating point. Controllers are designed on the linear model and **verified on the nonlinear model by simulation**. [Stated §6.4] |
| Feedback linearization | Use a nonlinear feedback law that cancels the nonlinearity, so the closed loop is exactly linear. [Stated §6.4] |

**Examples:** cruise control (6.11, 6.12, 6.14), vehicle steering (6.13).

**Prerequisites:** Ch. 5 (equilibrium points, linearization for stability); partial derivatives; Module A (what "linear" means).

---

## 3. Dependency Map

```
Ch. 3: dx/dt = Ax + Bu, y = Cx + Du       Ch. 5: equilibria, eigenvalue stability test
        │                                          │
        ▼                                          │
Module A: Linearity (superposition), time invariance
        │                                          │
        ▼                                          │
Module B: Matrix exponential e^{At} (Prop 6.1)     │
   → diagonalization / Jordan form (Thm 6.2)       │
   → eigenvalues & modes  ─────────────────────────┤ (justifies Thm 5.1)
        │                                          │
        ▼                                          │
Module C: Convolution equation (Thm 6.4)           │
   ├→ impulse response                             │
   ├→ coordinate invariance                        │
   ├→ transient vs steady state → step response specs → DC gain
   ├→ frequency response → gain, phase, bandwidth, resonant peak → (Bode, Ch. 9)
   └→ sampling (discrete-time model)               │
                                                   ▼
Module D: Linearization (needs: equilibria from Ch. 5 + A/B/C/D form + partial derivatives)
   ├→ Jacobian linearization
   └→ feedback linearization (needs: Jacobian idea + feedback concept)
```

**Must be understood first:** superposition (A) → *e^{At}* (B1–B2) → the convolution equation (C1).

**Can be learned independently:**
- **Module D (linearization) can be learned right after Ch. 5**, in parallel with Modules B–C. It needs only equilibria and partial derivatives. [Inferred]
- Sampling (C11) is a side branch. The continuous-time chapters 7–14 do not need it.
- Jordan form (B5) and Proposition 6.3 can be postponed. They matter only for repeated eigenvalues.

**Depend on several earlier concepts:**
- The frequency response (C8) needs the convolution equation (C1), the steady-state idea (C4), **and** complex numbers.
- The DC gain formula (C7) needs the steady-state response (C4) **and** an invertible *A* (that is, stability).
- Linearization validity (D) needs equilibrium points (Ch. 5) **and** local stability (Theorem 5.3).

**Forward links [Inferred]:**
- Eigenvalues → Ch. 7 (where state feedback moves them).
- Frequency response → Ch. 9 (transfer function = frequency response evaluated at *s*), Ch. 10 (Nyquist).
- Step response specs → Ch. 7 (design specs), Ch. 11 (PID tuning), Ch. 12 (performance specs).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Superposition; *x(t) = e^{At}x(0)*; eigenvalues ↔ modes ↔ stability; the convolution equation (what each term means); step response and its specs (rise time, overshoot, settling time, steady-state value); DC gain; frequency response (gain and phase); equilibrium/operating point; Jacobian linearization |
| **Supporting** | Power series definition of *e^{At}*; diagonalization; coordinate invariance; impulse response; bandwidth and resonant peak; validity range of linearization; double integrator and oscillator examples |
| **Advanced / postpone** | Jordan form and Proposition 6.3; sampling (Φ, Γ); feedback linearization; coupled spring–mass mode shapes; active filter circuit details |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Linear algebra:** eigen-decomposition *A = TΛT⁻¹*; matrix inverse; matrix power series. This chapter leans on linear algebra heavily. [Stated, Preface: linear algebra including eigenvalues is assumed]
- **ODEs:** homogeneous plus particular solutions. [Stated, Preface: assumed]
- **Integration:** integrals with a variable upper limit; the idea of the Dirac delta (for the impulse response).
- **Complex numbers:** *e^{iωt} = cos ωt + i sin ωt*; magnitude and argument.
- **Multivariable calculus:** partial derivatives and the Jacobian matrix.

### Terms that need clarification
| Term | Issue |
|---|---|
| "Linear" for a system with *x(0) ≠ 0* | An *affine* map like *y = ax + b* is **not** linear in the strict sense. The book handles this by treating *(x(0), u)* jointly. |
| Gain | Here it means the frequency response magnitude *\|G(iω)\|* or the DC gain. Different from the static gain *k* of Ch. 2. |
| Phase | The angle of *G(iω)*. Negative phase means the output lags. |
| Mode | Eigenvector direction plus its eigenvalue time behavior. Used both in vibration language and in linear algebra language. |
| Operating point vs equilibrium point | Used almost interchangeably in §6.4 for the pair *(x_e, u_e)*. |
| Impulse | An idealized "infinitely short, unit-area" input. A mathematical idealization, not a physical signal. |

### Topics the chapter does not explain enough
- **How to compute *e^{At}* by hand** for non-diagonalizable *A*: only sketched via the Jordan form. The book refers to linear algebra texts (Strang).
- **Choosing a sampling period** and aliasing: §6.3 gives only the exact discretization. Practical sampling advice is in Ch. 11 §11.5.
- **How large the "neighborhood" is** where the linearization is valid: no quantitative answer is given; the advice is "verify by simulation".
- **Feedback linearization** is shown only by example. The general theory needs nonlinear control texts.
- **Laplace transforms** are *not* used. The frequency response is derived from sinusoidal inputs. If you know Laplace, connect it yourself, or see §9.3. [Stated, Preface 1st ed.]

### Note for Drillbotics [Inferred]
- Module D is how you will get linear models of the **Simscape motor/drillstring plant** around a nominal (WOB, RPM) operating point. Simulink's `linearize` tool performs Jacobian linearization.
- Module C's **sampling** is what the controller sees once it runs on real I/O (toy drill or lab rig), because it then runs at a fixed sample rate.
