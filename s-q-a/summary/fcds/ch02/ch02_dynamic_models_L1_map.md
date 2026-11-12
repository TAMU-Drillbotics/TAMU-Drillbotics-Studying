# Chapter 2: Dynamic Models — Layer 1 Knowledge Map

> Source: Franklin, Powell & Emami-Naeini, *Feedback Control of Dynamic Systems*, 7th ed., Chapter 2, pp. 23–83.
> Layer: 1 (knowledge map). What to learn and in what order, not full explanations.
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **how to write the differential equations (the "dynamic model") of a physical system** from the laws of physics. To control engineers, a model "means a set of differential equations that describe the dynamic behavior of the process." [Stated, Perspective] The chapter covers mechanical, electrical, electromechanical, thermal, and fluid systems.

### What problems does it address?
- Modeling is "the first step" of control design: you cannot analyze or design a controller without a model. [Stated, Summary]
- How to get equations of motion systematically (free-body diagrams + Newton's laws).
- How to model circuits, motors, and fluid/thermal systems with the same mindset.
- How to deal with nonlinear equations: approximate them with linear ones (small-angle, small-signal), or simulate them numerically (Simulink). [Stated, Chapter Overview]

### Major components
1. What a model is, and where it comes from (Perspective)
2. Mechanical systems: translation, rotation, combined motion, flexible bodies (§2.1) ★
3. Electric circuits, including the operational amplifier (§2.2)
4. Electromechanical systems: loudspeaker, DC motor, gears (§2.3) ★
5. Heat-flow and fluid-flow models (§2.4, optional)
6. Cross-cutting tools introduced early: transfer function, step response in Matlab, Simulink, linearization

### After this chapter you should be able to:
- Draw a free-body diagram and write the equations of motion for one or two masses (e.g., a car suspension).
- Write rotational equations (*M* = *Iα*) for a satellite or a pendulum, and linearize the pendulum for small angles.
- Write circuit equations with Kirchhoff's laws and model an ideal op-amp circuit.
- Write the coupled electrical and mechanical equations of a DC motor using the torque constant and back-emf constant.
- Recognize that a nonlinear model can be linearized near an operating point or simulated directly.

---

## 2. Knowledge Structure

### Module A — What a model is (Perspective, Chapter Overview)
**Purpose:** Set expectations before writing any equations.

| Concept | One-line gloss |
|---|---|
| Dynamic (mathematical) model | A set of differential equations describing how the process behaves over time. [Stated, Perspective] |
| Physics-based modeling | Derive the equations from physical laws. This chapter's focus. [Stated, Perspective] |
| System identification | Build a model from measured input/output data of a prototype. Only briefly described (Ch. 3 / web). [Stated, Perspective] |
| Model verification | "A careful control system designer will typically rely on at least some experiments to verify the accuracy of the model." [Stated, Perspective] |
| Linear emphasis | Real models are often nonlinear; linear models are "usually adequate for purposes of control design," so the early chapters focus on them. Full nonlinear treatment is in Ch. 9. [Stated, Chapter Overview] |

**Why together:** The philosophy of modeling, before any specific physics.

**Prerequisites:** Ch. 1 (process, plant, input, output).

---

### Module B — Mechanical systems (§2.1) ★
**Purpose:** The main modeling skill of the chapter: getting equations of motion for rigid bodies.

Learn these in order:

| # | Concept | One-line gloss |
|---|---|---|
| 1 | Newton's law (translation) | *F* = *ma*: the vector sum of forces on a body equals mass × acceleration. [Stated §2.1.1] |
| 2 | Free-body diagram | A sketch of one body with every force acting on it and its reference direction. [Stated §2.1.1] |
| 3 | First example: cruise control | A one-mass model with friction gives a first-order ODE (Ex. 2.1). [Stated §2.1.1] |
| 4 | Transfer function (first look) | Writing the ODE with *s* in place of *d/dt* gives an input/output ratio, the transfer function. Used to get a step response in Matlab. Formal treatment in Ch. 3. [Stated §2.1.1] |
| 5 | Two-mass system | Car suspension ("quarter car") model (Ex. 2.2). Each mass gets its own free-body diagram. [Stated §2.1.1] |
| 6 | Newton's law (rotation) | *M* = *Iα*: sum of moments = moment of inertia × angular acceleration. [Stated §2.1.2] |
| 7 | Double-integrator plant | Satellite attitude: torque → angle is 1/(*Is*²) (Ex. 2.3). This plant returns in Ch. 5–7. [Stated §2.1.2] |
| 8 | Flexibility; collocated vs noncollocated | Disk-drive read/write head with a flexible arm (Ex. 2.4). **Collocated** = sensor and actuator on the same rigid body; **noncollocated** = flexibility between them, which is "far more difficult to control." [Stated §2.1.2] |
| 9 | Pendulum and small-angle linearization | Pendulum equation contains sin θ. For small θ, sin θ ≈ θ gives a linear model. [Stated §2.1.2] |
| 10 | Nonlinear simulation | Simulink simulates the full nonlinear pendulum, to compare with the linear model. [Stated §2.1.2] |
| 11 | Combined rotation and translation | Hanging crane; linearizing near θ = π gives the inverted pendulum (Ex. 2.7). [Stated §2.1.3] |
| 12 | Distributed-parameter systems | Flexible bodies (beams) have infinitely many modes; approximate them with a few rigid bodies and springs = **lumped-parameter model**. [Stated §2.1.5] |
| 13 | The 5-step procedure | (1) Choose position variables, (2) free-body diagram of each body, (3) apply Newton (translation/rotation), (4) eliminate internal forces, (5) check #equations = #unknowns. [Stated §2.1.6] |

**Why together:** All of these apply Newton's laws to bodies that move.

**Prerequisites:** Module A; Newtonian mechanics (forces, moments, inertia); ODE notation (ẋ, ẍ).

---

### Module C — Electric circuits (§2.2)
**Purpose:** Model circuits, which appear both as plants and as analog controllers.

| Concept | One-line gloss |
|---|---|
| Circuit elements | Resistor, capacitor, inductor, and voltage/current sources with their voltage–current laws. [Stated §2.2] |
| Kirchhoff's current law (KCL) | The currents into a node sum to zero. [Stated §2.2] |
| Kirchhoff's voltage law (KVL) | The voltages around a closed loop sum to zero. [Stated §2.2] |
| Node analysis | Write KCL at each node to get the circuit equations. [Stated §2.2] |
| Operational amplifier (ideal) | Very high gain and **zero input current**, which makes op-amp circuits easy to analyze. [Stated §2.2, Review Q 2.7] |
| Op-amp circuits | Summer (Ex. 2.10), integrator, and others. These are how analog controllers are built. [Stated §2.2] |

**Why together:** Same goal as Module B (get ODEs), but using Kirchhoff's laws instead of Newton's.

**Prerequisites:** Module A; basic circuit theory.

**Independent of Module B.** You can learn circuits and mechanics in either order. [Inferred]

---

### Module D — Electromechanical systems (§2.3) ★
**Purpose:** Couple electrical and mechanical equations. This is the model of most actuators.

| Concept | One-line gloss |
|---|---|
| Law of motors | Force on a current-carrying conductor in a magnetic field: *F* = *Bli*. [Stated §2.3.1] |
| Law of generators | Voltage induced in a moving conductor: *e* = *Blv*. [Stated §2.3.1] |
| Loudspeaker | First example combining both laws with a circuit (Ex. 2.13). [Stated §2.3.1] |
| DC motor: torque constant *K*ₜ | Motor torque = *K*ₜ × armature current. [Stated §2.3.2] |
| DC motor: back-emf constant *K*ₑ | Voltage generated = *K*ₑ × shaft speed; it opposes the applied voltage. [Stated §2.3.2] |
| DC motor model | Armature circuit (KVL) + rotor dynamics (*M* = *Iα*), coupled through *K*ₜ and *K*ₑ. [Stated §2.3.2] |
| Gears (optional ▲) | Gear ratio *n* multiplies torque and divides speed; the load inertia reflected to the motor changes by *n*². [Stated §2.3.3] |

**Why together:** Each converts between electrical and mechanical energy.

**Prerequisites:** Module B (rotational motion) **and** Module C (KVL).

---

### Module E — Heat and fluid flow (§2.4, optional ▲)
**Purpose:** Show that thermal and fluid systems also produce ODEs.

| Concept | One-line gloss |
|---|---|
| Heat flow | *q* = (*T*₁ − *T*₂)/*R*: heat flow is driven by a temperature difference through a thermal resistance. [Stated §2.4.1] |
| Thermal capacitance | Heat stored raises temperature; *C* = *mc*ᵥ. [Stated §2.4.1] |
| Continuity (conservation of mass) | Rate of change of stored mass = inflow − outflow. [Stated §2.4.2] |
| Fluid resistance | Flow through a restriction depends on pressure difference, often nonlinearly (square root). [Stated §2.4.2] |
| Water-tank model and its linearization | Tank height ODE (Ex. 2.17), linearized about an operating point (Ex. 2.19). [Stated §2.4.2] |
| Hydraulic actuator | A piston driven by fluid pressure (Ex. 2.20). [Stated §2.4.2] |

**Why together:** Same pattern (storage + flow) in two new physical domains.

**Prerequisites:** Module A; basic thermodynamics and fluid mechanics.

---

## 3. Dependency Map

```
Module A: What is a model?  (ODEs; physics vs experiment; linear emphasis)
      │
      ├──────────────────────────┬─────────────────────────────┐
      ▼                          ▼                             ▼
Module B: Mechanical ★     Module C: Circuits           Module E: Heat & fluid (optional)
  F=ma → free-body diagram   KCL, KVL → node analysis     heat flow, storage
  → 1-mass, 2-mass           → op-amp (ideal)             continuity, resistance
  M=Iα → satellite (1/s²)                                  → tank linearization
       → pendulum → small-angle linearization
       → crane / inverted pendulum
  flexibility → collocated vs noncollocated
  distributed → lumped
      │                          │
      └────────────┬─────────────┘
                   ▼
        Module D: Electromechanical ★  (laws of motors/generators → DC motor → gears)
```

**Must be understood first:** what a dynamic model is; free-body diagram + Newton's law.

**Can be learned independently:**
- Circuits (C) and mechanics (B) do not depend on each other.
- Heat and fluid (E) depends only on Module A.
- Gears (D, ▲) can be skipped without blocking anything else in this chapter.

**Depend on several earlier concepts:**
- DC motor (D) needs rotational dynamics (B) **and** KVL (C).
- Linearization (B9, E) needs an equation of motion **and** an operating point (an equilibrium) to linearize about.
- Collocated vs noncollocated (B8) needs two-body models (B5) **and** rotation (B6); its control consequences are shown in Ch. 5 (§5.3) and Ch. 10 (§10.2).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Model = set of ODEs; free-body diagram; *F* = *ma*, *M* = *Iα*; the 5-step procedure; one- and two-mass systems; double-integrator satellite; pendulum small-angle linearization; DC motor equations (*K*ₜ, *K*ₑ) |
| **Supporting** | KCL, KVL, ideal op-amp; collocated vs noncollocated; Simulink nonlinear simulation; transfer function (first look); gears; loudspeaker |
| **Advanced / postpone** | Distributed-parameter and lumped models; heat and fluid models; hydraulic actuator; complex mechanical systems (web, Appendix W2.1.4); history (§2.5) |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Newtonian mechanics:** forces, moments, moment of inertia, springs (*kx*), dampers (*bẋ*).
- **Circuit theory:** Ohm's law, capacitors (*i* = *C dv/dt*), inductors (*v* = *L di/dt*).
- **ODE notation:** first and second derivatives; what an ODE "solution" is.
- **Small-angle approximations:** sin θ ≈ θ, cos θ ≈ 1.

### Terms that need clarification
| Term | Issue |
|---|---|
| "Transfer function" | Used in §2.1 before it is defined. Treat it as "the ODE written with *s* for *d/dt*" until Ch. 3 §3.1.2 defines it properly. |
| "Equations of motion" | Another name for the dynamic equations of a mechanical system. [Stated §2.1, footnote] |
| "Collocated" | About **where** the sensor and actuator are on a flexible structure, not about the controller. |
| "Back emf" | The voltage a spinning motor generates, which opposes the supply voltage. It acts like built-in velocity feedback. [Inferred] |
| "Linearize" | Replace a nonlinear function by its straight-line (tangent) approximation near an operating point. Only shown by examples here; Ch. 9 §9.2 formalizes it. |

### Topics the chapter does not explain enough
- **How to solve the ODEs** is deferred to Ch. 3. [Stated, Chapter Overview]
- **State-space form** (first-order vector equations) is not used here; it appears in Ch. 7. FBS Ch. 3 teaches modeling directly in state-space form. [Inferred comparison]
- **Linearization** is example-based only; the general method (Jacobians at an equilibrium) is in Ch. 9 §9.2.
- **System identification** is mentioned but not taught (web Appendix W3.7).
- **Friction models** beyond linear viscous damping are not covered here (Ch. 9 touches on nonlinear friction).

### Note for Drillbotics [Inferred]
- The **DC motor model** (§2.3.2) and **gears** (§2.3.3) are the base model for top-drive rotation and feed (hoist) actuators. Simscape's motor blocks implement exactly these equations.
- A long drill string is a **flexible, distributed-parameter** body (§2.1.5). Torsional stick-slip comes from that flexibility combined with nonlinear bit friction. A lumped model (a few inertias and torsional springs) is the usual first approximation.
- Sensor at the surface, bit at the bottom = a **noncollocated** arrangement (§2.1.2), which the book warns is harder to control.
