# Chapter 9: Nonlinear Systems — Layer 1 Knowledge Map

> Source: Franklin, Powell & Emami-Naeini, *Feedback Control of Dynamic Systems*, 7th ed., Chapter 9, pp. 637–702.
> Layer: 1 (knowledge map). What to learn and in what order, not full explanations.
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **what to do when the plant or controller is not linear**. "All systems are nonlinear, especially if large signals are considered," but most can be approximated by linear models for small signals. [Stated, Perspective] The chapter explains why the linear methods of Ch. 3–8 are still justified, and gives tools for the cases where nonlinear effects (saturation, friction, relays, hysteresis) must be taken into account.

### What problems does it address?
- Why is it valid to design on a linear model? (Lyapunov's first method.) [Stated §9.1]
- How to get a linear model: small-signal linearization, inverse nonlinearity, feedback linearization. [Stated §9.2]
- How a **saturating actuator** changes the behavior of a loop, including **integrator windup**. [Stated §9.3]
- How to predict **limit cycles** (self-sustained oscillations). [Stated §9.3–9.4]
- How to *prove* stability for a nonlinear system (Lyapunov's second method, circle criterion). [Stated §9.5]

### Major components [Stated, Chapter Overview: "four categories"]
1. Motivation (§9.1)
2. Reduce to a linear model: linearization (§9.2) ★
3. Heuristic: nonlinearity as a varying gain + root locus; antiwindup (§9.3) ★
4. Heuristic: describing functions + Nyquist (§9.4)
5. Rigorous: phase plane, Lyapunov stability, circle criterion (§9.5, optional ▲)

### After this chapter you should be able to:
- Linearize a nonlinear ODE about an equilibrium.
- Explain why a stable linearization guarantees local stability of the nonlinear system (and an unstable one guarantees instability).
- Treat saturation as a gain that falls as the signal grows, and predict its effect from a root locus.
- Explain integrator windup and draw an antiwindup scheme for a PI controller.
- Compute or look up a describing function and use it with a Nyquist plot to predict a limit cycle.
- Describe what a Lyapunov function is and what the circle criterion guarantees.

---

## 2. Knowledge Structure

### Module A — Motivation (§9.1, Perspective)
**Purpose:** Justify linear design, and explain where it fails.

| Concept | One-line gloss |
|---|---|
| Small-signal validity | Near an equilibrium and for small signals, most systems are nearly linear (sin θ ≈ θ). [Stated, Perspective] |
| Lyapunov's first method | If the linear approximation is strictly stable (all roots in the LHP), the nonlinear system is stable in some region around the equilibrium; if it has an RHP root, the nonlinear system is unstable there. [Stated §9.1] |
| When nonlinear effects matter | Large signals that saturate devices, or nonlinearities active even for small signals (some kinds of friction). [Stated, Perspective] |
| Heuristic vs rigorous methods | Equivalent gain and describing functions give insight but **cannot guarantee** stability; Lyapunov-type methods can. [Stated, Chapter Overview] |
| Internal vs external stability tests | Lyapunov uses state equations (internal); the circle criterion uses the frequency response (external). [Stated §9.1] |

**Prerequisites:** Ch. 3 (stability by poles); Ch. 2 (pendulum linearization).

---

### Module B — Analysis by linearization (§9.2) ★
**Purpose:** Three ways to get a linear model.

| Concept | One-line gloss |
|---|---|
| Equilibrium (operating point) | A constant state and input where all derivatives are zero. [Stated §9.2.1] |
| Small-signal linearization | Expand the nonlinear equations about the equilibrium and keep first-order terms (partial derivatives), giving a linear model of the **deviations**. [Stated §9.2.1] |
| Linearization by nonlinear feedback | Use part of the control to cancel the nonlinear terms, then design the rest linearly; in robotics this is **computed torque**. [Stated §9.2.2] |
| Linearization by inverse nonlinearity | Put the inverse of a known static nonlinearity (e.g., a lamp's power curve) in series before it, so the combination is linear. [Stated §9.2.3, Review Q 9.3] |

**Prerequisites:** Module A; calculus (derivatives, Taylor expansion, partial derivatives); Ch. 2 models.

---

### Module C — Equivalent gain with the root locus (§9.3) ★
**Purpose:** Treat a memoryless nonlinearity as a gain that depends on signal size.

| Concept | One-line gloss |
|---|---|
| Memoryless (zero-memory) nonlinearity | Output depends only on the current input: saturation, relay, relay with dead zone, gain with dead zone (Fig. 9.6). [Stated §9.3] |
| Equivalent gain | The ratio output/input; for saturation it falls as the input grows. [Stated §9.3] |
| Root-locus interpretation | Plot the locus vs this gain; as signal size changes, the closed-loop roots slide along the locus. [Stated §9.3] |
| Conditionally stable + saturation | A loop stable only for a range of gains can go unstable for **large** signals, because saturation lowers the effective gain. [Stated §9.3] |
| Limit cycle | A periodic oscillation of fixed amplitude; predicted where the equivalent gain puts roots on the *j*ω axis. [Stated §9.3] |
| Integrator windup | During saturation, the integrator keeps accumulating error ("winds up"), causing large overshoot when the error finally changes sign. [Stated §9.3.1, Review Q 9.4] |
| Antiwindup | Local feedback around the integrator that keeps its input near zero while the actuator is saturated; reduces overshoot and control effort (Ex. 9.9). [Stated §9.3.1] |

**Prerequisites:** Ch. 5 (root locus); Ch. 4 (PI control); Module A.

---

### Module D — Describing functions (§9.4)
**Purpose:** An approximate "frequency response" for a nonlinear element, so Nyquist-type analysis can be used.

| Concept | One-line gloss |
|---|---|
| Describing function *N*(*a*) | Apply a sinusoid of amplitude *a*; keep only the **first harmonic** of the output; *N* = (first harmonic)/(input). It depends on amplitude (and, with memory, on frequency). [Stated §9.4, Chapter Overview] |
| Common describing functions | Saturation, relay, hysteresis, backlash. [Stated §9.4] |
| Stability analysis | Plot the linear part's Nyquist plot and −1/*N*(*a*); an intersection predicts a limit cycle and its amplitude and frequency. [Stated §9.4.1, Summary] |
| Heuristic status | "Essentially a heuristic method"; gives the same results as the equivalent-gain root locus for memoryless nonlinearities. [Stated, Summary] |
| Assumptions | Review Q 9.8 asks for them. Standard answer: a single nonlinearity, and a linear part that filters out higher harmonics (low-pass). [Stated question; answer Inferred] |

**Prerequisites:** Ch. 6 (Nyquist plot, frequency response); Fourier series (first harmonic); Module C.

---

### Module E — Analysis and design based on stability (§9.5, optional ▲)
**Purpose:** Rigorous tools.

| Concept | One-line gloss |
|---|---|
| Phase plane | Plot the trajectory of two state variables (e.g., position vs velocity) to see nonlinear behavior directly. [Stated §9.5.1, Summary] |
| Bang-bang (minimum-time) control | With bounded control, the fastest response switches between the limits; the switch is decided by a **switching curve** in the phase plane. [Stated §9.5.1] |
| Lyapunov's second method | Find a scalar, energy-like function *V*(*x*) > 0 whose derivative along trajectories is negative; then the state goes to the equilibrium. A **sufficient** condition. [Stated §9.1, §9.5.2] |
| Lyapunov equation (linear case) | For stable *A* and any positive definite *Q*, the solution *P* of *A*ᵀ*P* + *PA* = −*Q* is positive definite and gives *V* = *x*ᵀ*Px*. [Stated §9.5.2] |
| Lyapunov redesign / adaptive control | Use a Lyapunov function to choose an adaptation law that keeps a model-reference adaptive controller stable. [Stated §9.5.2] |
| Circle criterion | For a linear system with one nonlinearity in a **sector**, a condition on the Nyquist plot (avoid a circle) guarantees stability. A sufficient condition. [Stated §9.5.3, Summary] |

**Prerequisites:** Ch. 7 (state-space form, eigenvalues); Module A; positive-definite matrices; Ch. 6 (Nyquist) for the circle criterion.

---

## 3. Dependency Map

```
Ch.2 models, Ch.3 stability (poles)
          │
          ▼
Module A: all systems nonlinear; small-signal validity; Lyapunov's first method;
          heuristic vs rigorous
          │
          ▼
Module B ★: equilibrium → small-signal linearization
            (inverse nonlinearity and feedback linearization are side branches)
          │
    ┌─────┴───────────────────────────┬──────────────────────────────┐
    ▼                                 ▼                              ▼
Module C ★ (needs Ch.5, Ch.4)    Module D (needs Ch.6)          Module E ▲ (needs Ch.7)
 memoryless nonlinearity          first harmonic → N(a)          phase plane → bang-bang
 → equivalent gain → root locus   → −1/N(a) vs Nyquist           Lyapunov 2nd method
 → limit cycles                   → limit-cycle prediction       → Lyapunov equation
 → windup → antiwindup                                           circle criterion (needs Ch.6)
```

**Must be understood first:** equilibrium and small-signal linearization; why a stable linearization implies local stability.

**Can be learned independently:**
- Antiwindup (C) needs only PI control and saturation; it can be learned before the rest of the chapter.
- Inverse-nonlinearity and feedback linearization (B) do not depend on each other.
- The phase plane (E) does not need describing functions (D).

**Depend on several earlier concepts:**
- Describing-function stability analysis (D) needs the Nyquist criterion (Ch. 6) **and** the equivalent-gain idea (C).
- Saturation-induced instability (C) needs conditional stability (Ch. 5) **and** equivalent gain (C).
- The circle criterion (E) needs the Nyquist plot (Ch. 6) **and** Lyapunov's ideas (E).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | All real systems are nonlinear; equilibrium; small-signal linearization; Lyapunov's first method (the result); saturation; equivalent gain; integrator windup; antiwindup |
| **Supporting** | Inverse nonlinearity; limit cycles; conditionally stable systems with saturation; describing functions for saturation, relay, hysteresis; Nyquist with −1/*N*(*a*); phase plane |
| **Advanced / postpone** | Feedback linearization (computed torque); backlash describing function; bang-bang control and switching curves; Lyapunov's second method and the Lyapunov equation; Lyapunov redesign of adaptive control; circle criterion; history (§9.6) |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Calculus:** Taylor series, partial derivatives, Jacobian matrices (for multi-state linearization).
- **Fourier series:** the first harmonic of a periodic signal (for describing functions).
- **Positive-definite matrices** (for Lyapunov functions).
- Ch. 5 root locus, Ch. 6 Nyquist, Ch. 7 state form.

### Terms that need clarification
| Term | Issue |
|---|---|
| "Memory" in a nonlinearity | No memory = output depends only on the present input (saturation). With memory = depends on past (hysteresis, backlash). |
| Equivalent gain vs describing function | Equivalent gain is for memoryless elements on a root locus; the describing function generalizes it to elements with memory, using Nyquist. |
| Limit cycle | A **nonlinear** periodic oscillation with a definite amplitude, unlike the amplitude-free oscillation of a neutrally stable linear system. |
| Lyapunov's first vs second method | First = stability from the linearization (local). Second = construct an energy-like function (can give a region of stability). |
| Sufficient condition | Lyapunov and circle-criterion results guarantee stability if satisfied; failing them proves nothing. [Stated §9.1] |

### Topics the chapter does not explain enough
- **Region of attraction:** the size of the region where linearization-based stability holds is not given by the linear model. [Stated §9.1]
- **Friction models** (static/Coulomb/stick-slip) are mentioned as a motivation but not modeled in depth. [Inferred]
- **Adaptive control** appears only as an example inside §9.5.2 ("Lyapunov redesign" of a model-reference adaptive controller), not as its own section. Treat it as a preview. [Stated §9.5.2; the "preview" judgment is Inferred]
- The book itself says nonlinear control is "a vast and sophisticated topic" and this is only "a brief introduction." [Stated §9.1]

### Note for Drillbotics [Inferred]
- **Actuator limits are mandatory in the team plan**, so Module C (saturation + **antiwindup**) is essential for every PI/PID loop on the rig.
- **Stick-slip** is a friction-driven limit cycle. Module D (describing functions) and the phase plane (E) are the classical tools for thinking about it.
- Linearize the drilling model (Module B) at the planned operating point (target WOB/RPM) before using any Ch. 4–7 design method; re-check at other operating points.
- Hydraulic valves and motor drives often have **dead zones** and **backlash** (Module C/D).
