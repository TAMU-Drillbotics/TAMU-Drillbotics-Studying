# Chapter 5: Dynamic Behavior — Layer 1 Knowledge Map

> Source: Åström & Murray, *Feedback Systems*, 2nd ed., Chapter 5, pp. 5-1 to 5-34.
> Layer: 1 (knowledge map).
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **how solutions of (mostly nonlinear) differential equations behave over time**: where they settle, whether they oscillate, whether nearby solutions stay nearby, and how this changes when parameters change. [Stated, chapter intro]

### What problems does it address?
- Does the model even have a solution, and is it unique? (§5.1)
- How can we *see* the behavior of a 2-state system without solving it? (§5.2: phase portraits)
- What exactly does "stable" mean, and how do we test it? (§5.3)
- How can we prove stability for nonlinear systems, using energy-like functions? (§5.4: Lyapunov)
- What happens far from an equilibrium, or when a parameter changes? (§5.5)

### Why it matters for control
Chapter 1 named **instability** as the main danger of feedback. This chapter gives the **formal definition of stability** and the **tests** used for the rest of the book. [Inferred: the link to Ch. 1 is not stated in this chapter]

### Major components
1. Solutions of differential equations: existence and uniqueness (§5.1)
2. Qualitative analysis: phase portraits, equilibria, limit cycles (§5.2)
3. Stability: definitions, linear systems, linearization, limit cycles (§5.3) ★
4. Lyapunov stability analysis (§5.4). The book marks it as **advanced**. [Stated, Preface 1st ed.]
5. Parametric and nonlocal behavior: regions of attraction, bifurcations, nonlinear design (§5.5)

### After this chapter you should be able to:
- Find the equilibrium points of *dx/dt = F(x)*.
- Sketch or read a phase portrait, and spot equilibria and limit cycles.
- Define stable, asymptotically stable, and unstable (locally and globally).
- Use **Theorem 5.1**: a linear system *dx/dt = Ax* is asymptotically stable ⇔ all eigenvalues of *A* have negative real part.
- Linearize a nonlinear system at an equilibrium and use the eigenvalues to decide local stability (Theorem 5.3).
- (Advanced) Use a Lyapunov function to prove stability.

---

## 2. Knowledge Structure

### Module A — Solving differential equations (§5.1)
**Purpose:** Make sure the model is well-posed before studying its behavior.

| Concept | One-line gloss |
|---|---|
| State space system *dx/dt = f(x,u), y = h(x,u)* | Restated from Ch. 3. [Stated §5.1] |
| Solution / initial value problem | A function *x(t)* that satisfies the ODE and starts from *x(0) = x₀*. [Stated §5.1] |
| Existence and uniqueness | Smooth (Lipschitz) *F* ⇒ a unique solution exists, at least for a short time. [Stated §5.1] |
| Finite escape time | The solution blows up in finite time (Ex. 5.2: *dx/dt = x²*). [Stated §5.1] |
| Nonunique solutions | A non-smooth *F* can give several solutions from the same start (Ex. 5.3). [Stated §5.1] |
| Damped oscillator | Ex. 5.1: the standard 2nd-order linear example (*ζ, ω₀*). [Stated §5.1] |

**Prerequisites:** Ch. 3 state space form; calculus.

**Priority note:** Mainly a caveat. Most engineering models are smooth, so this rarely causes trouble. [Inferred]

---

### Module B — Qualitative analysis (§5.2)
**Purpose:** Understand behavior *geometrically*, without closed-form solutions.

| Concept | One-line gloss |
|---|---|
| Planar dynamical system | Two state variables, so trajectories can be drawn in the *(x₁, x₂)* plane. [Stated §5.2] |
| Vector field | At each point *x*, the arrow *F(x)* shows the direction of motion. [Stated §5.2] |
| Phase portrait | A plot of many trajectories in the state plane. [Stated §5.2] |
| Equilibrium point *x_e* | A state where *F(x_e) = 0*: the system stays there forever. [Stated §5.2] |
| Limit cycle | An isolated closed trajectory: a self-sustained oscillation. [Stated §5.2] |
| Inverted pendulum (Ex. 5.4) | Two kinds of equilibria: upright (unstable) and hanging (stable). [Stated §5.2] |

**Prerequisites:** Module A; Ch. 3 (state).

---

### Module C — Stability (§5.3) ★ the core of the chapter
**Purpose:** Give precise definitions and practical tests.

Learn in this order:

| # | Concept | One-line gloss |
|---|---|---|
| 1 | Stable (Lyapunov stable) solution | Start close ⇒ stay close (the ε–δ definition). [Stated §5.3] |
| 2 | Asymptotically stable | Stable **and** nearby solutions converge to it. [Stated §5.3] |
| 3 | Neutrally stable | Stable but not asymptotically stable (e.g., an undamped oscillator). [Stated §5.3] |
| 4 | Unstable | Not stable. [Stated §5.3] |
| 5 | Local vs global | Stability near the point vs for all initial conditions. [Stated §5.3] |
| 6 | Sink / source / saddle / center | Names for equilibrium types by phase portrait shape. [Stated §5.3] |
| 7 | **Theorem 5.1**: stability of *dx/dt = Ax* | Asymptotically stable ⇔ all eigenvalues of *A* have Re < 0; unstable if any has Re > 0. [Stated §5.3] |
| 8 | Eigenvalues and modes; diagonal / Jordan form | Each eigenvalue gives a mode *e^{λt}*; a change of coordinates can decouple the modes. [Stated §5.3] |
| 9 | Linearization at an equilibrium (Jacobian) | Approximate *F(x)* near *x_e* by *A(x − x_e)*. [Stated §5.3] |
| 10 | Stability via linear approximation | If the linearization is asymptotically stable, the equilibrium is **locally** asymptotically stable (formalized as Theorem 5.3). [Stated §5.3–5.4] |
| 11 | Stability of limit cycles | Do nearby trajectories approach the oscillation? (Ex. 5.9) [Stated §5.3] |

**Examples:** congestion control (Ex. 5.5), compartment model (Ex. 5.6), inverted pendulum (Ex. 5.7), tanker steering (Ex. 5.8).

**Why together:** Definitions (1–6) → test for linear systems (7–8) → extension to nonlinear systems by approximation (9–10) → stability of non-equilibrium solutions (11).

**Prerequisites:** Modules A and B; **linear algebra: eigenvalues and eigenvectors**; partial derivatives (for the Jacobian).

---

### Module D — Lyapunov stability analysis (§5.4) [book: advanced]
**Purpose:** Prove stability of nonlinear systems directly, using a generalization of energy. [Stated §5.4]

| Concept | One-line gloss |
|---|---|
| Lyapunov function *V(x)* | An energy-like function, positive away from the equilibrium, that does not increase along trajectories. [Stated §5.4] |
| Positive definite / semidefinite function | *V(0) = 0*, *V(x) > 0* (or ≥ 0) elsewhere. [Stated §5.4] |
| *dV/dt* along trajectories, *V̇ = (∂V/∂x) F(x)* | How *V* changes as the system moves. [Stated §5.4] |
| **Theorem 5.2** (Lyapunov stability theorem) | *V* > 0 and *V̇* ≤ 0 ⇒ stable; *V̇* < 0 ⇒ asymptotically stable. [Stated §5.4] |
| Quadratic Lyapunov functions, *V = xᵀPx* | The standard choice for linear systems (Lyapunov equation). [Stated §5.4] |
| **Theorem 5.3** (linearization) | Formal statement that eigenvalues of the linearization decide local stability. [Stated §5.4] |
| **Theorem 5.4** (Krasovski–LaSalle invariance principle) | Gets asymptotic stability even when *V̇* is only ≤ 0. [Stated §5.4] |
| Using Lyapunov functions to *design* a controller | Ex. 5.14: swing-up and stabilization of an inverted pendulum. [Stated §5.4] |

**Examples:** scalar system (5.10), hanging pendulum (5.11), spring–mass (5.12), genetic switch (5.13), inverted pendulum (5.14).

**Prerequisites:** Module C (definitions); multivariable calculus (gradient); positive definite matrices.

---

### Module E — Parametric and nonlocal behavior (§5.5)
**Purpose:** Look beyond "near one equilibrium, with fixed parameters". [Stated §5.5]

| Concept | One-line gloss |
|---|---|
| Region of attraction | The set of initial conditions that converge to a given equilibrium. [Stated §5.5] |
| Bifurcation | A qualitative change in behavior (equilibria appear or disappear, stability changes) as a parameter varies. [Stated §5.5] |
| Root locus (first appearance) | A plot of eigenvalues as a parameter varies. Ex. 5.17 shows bicycle stability vs speed. [Stated §5.5] |
| Design of nonlinear dynamics using feedback | Some problems need nonlinear controllers. Ex. 5.18: noise cancellation designed with a Lyapunov function. [Stated §5.5] |

**Prerequisites:** Modules C and D.

---

## 3. Dependency Map

```
Ch. 3 state space model  dx/dt = f(x,u)
        │
        ▼
Module A: Existence/uniqueness (caveats: finite escape, nonuniqueness)
        │
        ▼
Module B: Phase portraits → EQUILIBRIUM POINTS → limit cycles
        │
        ▼
Module C: STABILITY DEFINITIONS (stable / asymptotically / neutral / unstable; local / global)
        │
        ├──► Linear test: Theorem 5.1 (eigenvalues of A)  ◄── linear algebra: eigenvalues
        │          │
        │          ▼
        ├──► Linearization (Jacobian) → local stability of nonlinear equilibria
        │
        └──► Stability of limit cycles
        │
        ▼
Module D: Lyapunov functions (Thm 5.2) → linearization theorem (Thm 5.3)
          → Krasovski–LaSalle (Thm 5.4) → Lyapunov-based design
        │
        ▼
Module E: Regions of attraction (needs D) ; Bifurcations & root locus (needs C) ;
          Nonlinear design (needs D)
```

**Must be understood first:** equilibrium point → the stability definitions → Theorem 5.1 (eigenvalue test).

**Can be learned independently:**
- Module A (existence and uniqueness) can be skipped on a first pass. Later modules do not use it in practice. [Inferred]
- Bifurcations / root locus (Module E) need only Module C, not Lyapunov (Module D).
- Limit cycle stability (C11) is a side branch. Nothing in Ch. 6–8 depends on it.

**Depend on several earlier concepts:**
- Linearization-based stability (C9–C10) needs equilibrium points (B), the eigenvalue test (C7), **and** partial derivatives.
- Region of attraction (E) needs equilibria (B), the stability definitions (C), **and** Lyapunov functions (D).
- Lyapunov-based controller design needs Lyapunov functions (D) **and** the state space model with input (Ch. 3).

**Forward links (where this is used later) [Inferred from cross-references]:**
- Theorem 5.1 → Ch. 6 (matrix exponential), Ch. 7 (place eigenvalues by state feedback), Ch. 9 (poles = eigenvalues).
- Linearization → formalized for systems with inputs in §6.4.
- Root locus → §12.5.

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Equilibrium point; stable / asymptotically stable / unstable; local vs global; Theorem 5.1 (eigenvalues of *A* with Re < 0); linearization around an equilibrium; local stability from the linearization |
| **Supporting** | Phase portraits and vector fields; limit cycles; neutral stability; sink/source/saddle/center; idea of a Lyapunov function as "energy"; region of attraction; root locus as eigenvalues vs a parameter |
| **Advanced / postpone** | Existence and uniqueness, finite escape, nonunique solutions; ε–δ definitions in detail; Theorem 5.2 proof details; Lyapunov equation; Krasovski–LaSalle; stability of limit cycles; bifurcation theory; nonlinear design (noise cancellation); genetic switch |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Eigenvalues and eigenvectors** of a matrix, including **complex** eigenvalues. This is *critical* for Theorem 5.1. [Stated, Preface: linear algebra including eigenvalues is assumed]
- **Diagonalization** (change of basis), and what a **Jordan block** is (for repeated eigenvalues).
- **Partial derivatives and the Jacobian matrix** (for linearization).
- **Positive definite matrices** (for Lyapunov functions *xᵀPx*).
- **Taylor expansion** to first order.

### Terms that need clarification
| Term | Issue |
|---|---|
| "Stable" | The book's §2.2 meaning (the ODE solution decays) is what §5.3 calls *asymptotically stable*. In §5.3, "stable" alone is weaker: it only means "stays close". |
| Stability of a *solution* vs of a *system* | §5.3 defines stability for a solution or an equilibrium. For linear systems the two coincide, which is why people say "the system is stable". |
| Neutrally stable | Example: an undamped spring–mass system. It is stable but does not converge. |
| Local vs global | Linearization only ever gives **local** results. |
| Eigenvalue ↔ pole | They are the same numbers seen from two viewpoints. Ch. 9 makes the link explicit. |

### Topics the chapter does not explain enough
- **The proof of Theorem 5.1** relies on the matrix exponential and the Jordan form, which are fully developed only in §6.2.
- **How to find a Lyapunov function** for a new system: there is no general recipe. The book shows examples only.
- **The borderline case** where the linearization has eigenvalues with zero real part: Theorem 5.3 says nothing there, and the book only notes it.
- **Bifurcation theory and chaos** are only introduced. See §5.6 for references (e.g., Strogatz).
- **Numerical tools** for plotting phase portraits are not covered (MATLAB `quiver` and simulation are implied).

### Note for Drillbotics [Inferred]
**Stick-slip** in a drillstring is a **limit cycle**: a self-sustained torsional oscillation driven by nonlinear bit friction. Module B (limit cycles) and Module E (bifurcation: the oscillation appears above or below a certain RPM or WOB) are therefore more relevant to you than their "postpone" label suggests. Study them after the essentials.
