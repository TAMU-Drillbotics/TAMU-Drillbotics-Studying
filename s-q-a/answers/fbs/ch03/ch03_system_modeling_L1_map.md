# Chapter 3: System Modeling — Layer 1 Knowledge Map

> Source: Åström & Murray, *Feedback Systems*, 2nd ed., Chapter 3, pp. 3-1 to 3-44.
> Layer: 1 (knowledge map). What to learn and in what order, not full explanations.
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **how to write down a mathematical model of a dynamical system** so that it can be analyzed and simulated. The book's main modeling form is the **state space model**, written as an ordinary differential equation (continuous time) or a difference equation (discrete time). [Stated, chapter intro and §3.2]

### What problems does it address?
- What *is* a model, and why there is no single "correct" model of a system: the right model depends on the question you ask. [Stated, chapter intro]
- How to choose the **state**, the variables that summarize the past for predicting the future.
- How to put a model into a standard form, *dx/dt = f(x,u), y = h(x,u)*, so that the same tools work for any field.
- How to handle **large systems** (block diagrams), **measured systems** (models from experiments), and **messy units** (scaling).
- How to describe **uncertainty**, because a model is always an approximation.

### Major components
1. Modeling concepts and viewpoints (§3.1)
2. State space models: ODEs, difference equations, finite state machines, simulation (§3.2) ★
3. Modeling methodology: block diagrams, algebraic loops, models from experiments, scaling (§3.3)
4. Modeling examples from many fields (§3.4)

### After this chapter you should be able to:
- Define *state*, *input*, *output*, *order*, *linear*, *time-invariant*.
- Convert an *n*th-order ODE (e.g., a spring–mass system) into state space form, and write it in *A, B, C, D* form if it is linear.
- Tell continuous-time models (ODEs) from discrete-time ones (difference equations), and discretize a simple PI controller.
- Read and draw a block diagram, and recognize an algebraic loop.
- Explain why a model is valid only in some range, and why the book uses both state models and input/output (frequency) models.

---

## 2. Knowledge Structure

### Module A — Modeling concepts and viewpoints (§3.1)
**Purpose:** Explain what a model is and the two "heritages" that control theory merges.

| Concept | One-line gloss |
|---|---|
| Model | A mathematical representation used to answer questions by analysis and simulation. It always approximates the real system. [Stated §3.1] |
| State (mechanics heritage) | Variables such as position and velocity that determine future motion. [Stated §3.1] |
| Autonomous vs forced system | *dx/dt = F(x)* with no input vs *dx/dt = f(x, u)* with input *u*. [Stated §3.1] |
| Input/output view (electrical engineering heritage) | A system is a device that transforms inputs into outputs. [Stated §3.1] |
| Linearity (superposition) | Response to a sum of inputs = sum of responses. [Stated §3.1] |
| Time invariance | Shifting the input in time shifts the output by the same amount. [Stated §3.1] |
| Frequency response (first mention) | An LTI system is fully characterized by how it responds to sinusoids. [Stated §3.1] |
| The control view | Control merged both views: state models (from the late 1950s) plus the input/output view. [Stated §3.1] |
| Multidomain / acausal modeling, Modelica | Component-based, equation-based models across mechanical, electrical, thermal, and other domains. [Stated §3.1] |
| Hybrid (cyber-physical) system | Continuous dynamics plus discrete logic. Tools include Stateflow and Modelica. [Stated §3.1] |
| Model uncertainty | Nominal value plus a measure of uncertainty. The "uncertainty lemon" shows a model valid only in limited ranges of amplitude and frequency. Formalized in Ch. 13. [Stated §3.1] |

**Why together:** These are the *philosophy and vocabulary* of modeling before any formulas.

**Prerequisites:** Ch. 1 (dynamical system, feedback); Ch. 2 helps (linear ODE, frequency response).

**Key relationship [Stated §3.1]:** State models are convenient for describing a *nominal* model. *Uncertainty* is easier to describe with *input/output (frequency) models*. This is why the book uses both.

---

### Module B — State space models (§3.2) ★ the core of the chapter
**Purpose:** Give the standard model forms used in the rest of the book.

Learn these in order:

| # | Concept | One-line gloss |
|---|---|---|
| 1 | State *x* | "A collection of variables that summarize the past of a system for the purpose of predicting the future." Physically: storage of mass, momentum, energy. [Stated §3.2] |
| 2 | Input *u*, output *y* | What we can manipulate / what we measure or care about. [Stated §3.2] |
| 3 | State space model (nonlinear) | *dx/dt = f(x, u), y = h(x, u)*. [Stated §3.2, eq. 3.5] |
| 4 | Order | The dimension *n* of the state vector. [Stated §3.2] |
| 5 | Time-invariant model | *f* and *h* do not depend explicitly on time. [Stated §3.2] |
| 6 | Linear state space model (LTI) | *dx/dt = Ax + Bu, y = Cx + Du*. [Stated §3.2] |
| 7 | *A, B, C, D* names | Dynamics matrix, control matrix, sensor matrix, direct term. [Stated §3.2] |
| 8 | ODE → state space conversion | Choose *x₁ = y, x₂ = dy/dt, …*. Gives e.g. the **reachable canonical form**. [Stated §3.2] |
| 9 | Difference equations | *x[k+1] = f(x[k], u[k])*: evolution at discrete instants. [Stated §3.2] |
| 10 | Discretizing a controller | Example 3.6: a PI controller turned into a difference equation for a computer. [Stated §3.2] |
| 11 | Finite state machine | Discrete states plus transitions triggered by inputs (traffic light example). [Stated §3.2] |
| 12 | Simulation and analysis | Predicting state evolution from an initial condition, usually by numerical integration. [Stated §3.2] |

**Running examples introduced here (they are reused in later chapters):**
- Spring–mass system (Ex. 3.1, 3.8, 3.9)
- Balance systems: cart–pendulum (Ex. 3.2), inverted pendulum (Ex. 3.3)
- Predator–prey (Ex. 3.4, discrete time)
- E-mail server (Ex. 3.5)
- Traffic light FSM (Ex. 3.7)

**Why together:** These are all ways of writing "the current state plus the input determine what comes next", for continuous time, discrete time, and discrete values.

**Prerequisites:** Module A (state, input/output); math: vectors, matrices, and matrix–vector multiplication; ODEs; Newton's laws for the mechanical examples.

---

### Module C — Modeling methodology (§3.3)
**Purpose:** Practical techniques for building and managing models of real, large systems.

| Concept | One-line gloss |
|---|---|
| Graphical descriptions | Schematic or graphical representations that hide irrelevant details. [Stated §3.3] |
| Block diagram | Emphasizes **information flow**. Each block is a subsystem with inputs and outputs. [Stated §3.3] |
| Standard blocks | Summation, gain, saturation, nonlinear map, integrator, input/output (linear) system. [Stated §3.3, figure of elementary blocks] |
| Algebraic loop | A loop in a block diagram with no dynamics (no integrator) in it. It causes trouble when you form the equations or simulate. [Stated §3.3] |
| Modeling from experiments | Fit model parameters from measured input/output data (e.g., a step response of the spring–mass system). This gives input/output models only. [Stated §3.3] |
| Normalization and scaling | Use dimension-free variables to reduce the number of parameters and expose structure. [Stated §3.3] |

**Why together:** They are methods for *building* a model, as opposed to the model *forms* in Module B.

**Prerequisites:** Module B (state space models). Ch. 2 §2.2 (block diagram algebra).

**Relationship [Inferred]:** "Modeling from experiments" is the entry point to *system identification*. The book only sketches it. It matters for calibrating a simulator against real data.

---

### Module D — Modeling examples (§3.4)
**Purpose:** Show that the *same* state space machinery models systems in very different fields. [Stated §3.4]

| Domain | Examples |
|---|---|
| Motion control | Vehicle steering (Ex. 3.11), vectored thrust aircraft (Ex. 3.12) |
| Thermofluid | Water heater (Ex. 3.13), drum boiler (Ex. 3.14) |
| Information systems | Queuing systems (Ex. 3.15), virtual memory paging (Ex. 3.16), consensus in sensor networks (Ex. 3.17) |
| Biology | Transcriptional regulation (Ex. 3.18), nerve cells / Hodgkin–Huxley-type models (Ex. 3.19) |

**Why a module:** These are *instances* of Module B. Vehicle steering and vectored thrust aircraft come back as running examples in Ch. 5–8 and 12. [Stated: they recur in later chapters; the exact chapter list is Inferred from cross-references]

**Prerequisites:** Module B; physics or domain background for each example.

**Priority note:** Study one or two in depth (vehicle steering is the most reused). Skim the others.

---

## 3. Dependency Map

```
Module A: What is a model? (state vs input/output views, linearity, time invariance,
          uncertainty, hybrid systems)
      │
      ▼
Module B: State space models ★
  state → input/output → dx/dt = f(x,u), y = h(x,u) → order, time-invariant
      │                          │
      │                          └→ LINEAR: A, B, C, D  → ODE→state-space conversion
      │                                                     (reachable canonical form)
      ├→ difference equations → discretized PI controller
      ├→ finite state machines           (independent of the linear branch)
      └→ simulation
      │
      ├────────────────────────────┐
      ▼                            ▼
Module C: Methodology        Module D: Examples
  block diagrams → algebraic loops        (instances of B; pick 1–2)
  modeling from experiments
  normalization & scaling   (independent of the block-diagram items)
```

**Must be understood first:** *state*; the general form *dx/dt = f(x,u), y = h(x,u)*; the linear form *A, B, C, D*.

**Can be learned independently:**
- Finite state machines (B11) do not depend on the linear branch (B6–B8).
- Difference equations (B9) can be learned in parallel with ODEs once *state* is clear.
- Normalization and scaling and modeling from experiments (Module C) do not depend on each other.
- Within Module A, multidomain modeling and hybrid systems are side topics; nothing in Ch. 4–8 depends on them.

**Depend on several earlier concepts:**
- Discretized PI controller (B10) needs difference equations (B9) **and** PI control (Ch. 1–2).
- Algebraic loops (C) need block diagrams (C) **and** the ODE form (B3).
- Model uncertainty (A) is fully used only after Ch. 9–10 (frequency response) and Ch. 13.

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Model as an approximation; state; input/output; *dx/dt = f(x,u), y = h(x,u)*; order; linear and time-invariant; *A, B, C, D*; converting an ODE to state space; block diagram; spring–mass and balance-system examples |
| **Supporting** | Difference equations; discretizing a PI controller; superposition; frequency response (first mention); simulation; modeling from experiments; normalization and scaling; model uncertainty (qualitative); vehicle steering example |
| **Advanced / postpone** | Reachable canonical form (it returns in Ch. 7); finite state machines and hybrid systems; Modelica and acausal modeling; algebraic loops; thermofluid, information, and biological examples; the history of the mechanics and EE heritages |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Linear algebra:** vectors, matrices, matrix–vector products, the meaning of a vector-valued function *f: ℝⁿ × ℝᵖ → ℝⁿ*. [Stated, Preface: linear algebra is assumed]
- **ODEs:** how a 2nd-order ODE is the same as two 1st-order ODEs.
- **Newtonian mechanics:** *F = ma*, springs and dampers, torques, for the mechanical examples.
- **Basic circuits:** for the op-amp and electrical examples (more in Ch. 4).
- **Numerical integration** (what a simulator does): helpful but not required.

### Terms that need clarification
| Term | Issue |
|---|---|
| "State" vs "output" | The state is internal and may not be measurable. The output is what you see. Observability (Ch. 8) asks whether the outputs can reveal the state. |
| "Linear" | Linear in *x* and *u*, not "a straight-line plot". Superposition is the test. |
| "System" | Can mean a physical object, a model, or an input/output map, depending on context. |
| Reachable canonical form | Named here but explained in §7.1. Do not worry about it yet. |
| Acausal vs causal modeling | Block diagrams are causal (a signal goes *from* input *to* output). Modelica/Simscape models are acausal (equations with no fixed direction). The book only names the difference. |

### Topics the chapter does not explain enough
- **How to choose a state for a new system.** The book gives the "storage of mass, momentum, energy" rule and examples, but no full procedure.
- **Numerical simulation methods** (Euler, Runge–Kutta, step size) are not covered.
- **System identification** (fitting models from data) is only a short subsection. For serious use you need another source. [The book's §3.5 Further Reading gives references]
- **Model uncertainty** is qualitative here (the "uncertainty lemon"). The quantitative tools are in Ch. 13.
- **Discretization** is shown only for a PI controller. Sampling effects (aliasing, choosing the sample rate) are touched on in Ch. 11 §11.5 (implementation).

### Note for Drillbotics [Inferred]
- **Simscape is an acausal multidomain tool** of the Modelica kind (Module A). **Simulink is a causal block-diagram tool** (Module C). **Stateflow handles finite state machines and hybrid systems** (Module A/B). This chapter is the conceptual base for all three.
- "Modeling from experiments" (Module C) is the concept behind "calibrate the sim against real sensor data" in the team plan.
