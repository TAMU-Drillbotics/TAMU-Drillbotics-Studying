# Chapter 15: Architecture and System Design — Layer 1 Knowledge Map

> Source: Åström & Murray, *Feedback Systems*, 2nd ed. (electronic edition), Chapter 15, pp. 15-1 to 15-44.
> Layer: 1 (knowledge map).
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It places the single feedback loops of Chapters 1–14 inside **real, complete control systems**. It covers the **design process** (requirements → design → verification), **architecture** viewed **top-down** (layers of abstraction) and **bottom-up** (standard building blocks such as cascade and selector control), **interaction** between loops, **adaptation and learning**, and how control is done in major industries. [Stated, chapter intro]

### Position in the book
New in the 2nd (electronic) edition. Its aim is "a systems view that describes how control design is integrated into a larger model-based development framework". It also "briefly introduc[es] some of the many additional concepts from the field of control that are in widespread use in applications". [Stated, Preface 2nd ed.] Ch. 1 §1.8 previews this chapter. [Stated §1.8]

### What problems does it address?
- How is a control system designed and validated as part of a product? (§15.2: design V, model-based design)
- How do we split a complex control problem into layers (planning → trajectory generation → feedback)? (§15.3)
- What standard multi-loop structures exist, and when is each one used? (§15.4)
- What happens when loops interact, and how do we choose input–output pairings? (§15.5)
- How can controllers adapt and learn? (§15.6)

### Major components
1. Introduction: sensors, actuators, communication, computers, and operator interfaces in every system (§15.1)
2. System and control design: the design V, model-based design, commissioning (§15.2) ★
3. Top-down architectures: layered control, online optimization (MPC), linking continuous and discrete controllers, model checking (§15.3) ★
4. Bottom-up architectures: cascade, mid-range, selector, IMC, Smith predictor, complementary filtering, extremum seeking (§15.4) ★
5. Interaction: relative gain array (RGA), parallel systems (§15.5)
6. Adaptation and learning: adaptive control, learning, neural networks, deep learning (§15.6)
7. Control design in application fields: aerospace, automotive, process, telecom (§15.7)

### After this chapter you should be able to:
- Describe the design V and explain why model-based design moves verification earlier, where fixing faults is cheaper.
- Draw a layered control architecture (mission/decision logic → trajectory generation → feedback → actuators) for a system you know.
- Explain receding horizon control (MPC) at a conceptual level.
- Choose the right bottom-up structure for a situation: cascade (several sensors, one actuator), mid-range (several actuators), selector (constraints/protection), Smith predictor (delays), complementary filter (sensor fusion), extremum seeking (optimize an objective).
- Use the RGA to judge loop interaction and pick input–output pairings.
- Distinguish adaptation from learning, and place neural networks and deep learning within control.

---

## 2. Knowledge Structure

### Module A — Introduction (§15.1)
**Purpose:** Frame the chapter. [Stated §15.1]

| Concept | One-line gloss |
|---|---|
| Common elements of all control systems | Sensors, actuators, communications, computers, operator interfaces. [Stated §15.1] |
| Process and control co-design | Designing the process with control in mind has big advantages; the Wright Flyer is the example. [Stated §15.1–15.2] |

**Prerequisites:** Ch. 1 (§1.3 control loop; §1.8 architectures).

---

### Module B — System and control design (§15.2) ★
**Purpose:** Describe the overall engineering process in which controllers are developed. [Stated §15.2]

| Concept | One-line gloss |
|---|---|
| Understanding the system and its environment | Static and dynamic properties, sensors and actuators, safe operating bounds, disturbances, users. [Stated §15.2] |
| Design V (V-model) | Left leg: requirements → decomposition → design. Right leg: integration → verification → validation. [Stated §15.2] |
| Verification vs validation | "Building it right" vs "building the right thing". [Stated §15.2] |
| Cost of late faults | Faults found late cost dramatically more to fix. [Stated §15.2] |
| Model-based design | Models used as "virtual hardware" allow validation at many stages, not only at the end. [Stated §15.2] |
| Requirements expressed mathematically | So they can be checked automatically against models. [Stated §15.2] |
| Agile development | An alternative to the V for fast-changing requirements. [Stated §15.2] |
| Iterative control design workflow | Modeling → control design → simulation → testing on simulated models and real hardware → commissioning (including automatic tuning) → operation → upgrading. [Stated §15.2] |
| Hardware-in-the-loop | Testing controllers against simulated plants on real hardware. [Stated §15.2: mentioned] |

**Prerequisites:** Ch. 3 (modeling); general engineering design experience helps.

---

### Module C — Top-down architectures (§15.3) ★
**Purpose:** Break a complex control problem into a hierarchy of simpler problems at different layers of abstraction. [Stated §15.3]

| Concept | One-line gloss |
|---|---|
| Layered architecture | Each layer has its own specifications and makes assumptions about the layers above and below. [Stated §15.3, Fig. 15.5] |
| Typical layers | Top: decision-making / supervisory logic (mission, contingencies). Middle: trajectory generation / optimization with constraints. Bottom: feedback control. Plus layers of sensing and estimation. [Stated §15.3] |
| Bowtie / hourglass patterns | A common interface within a layer lets many components above and below change independently (as in the internet protocol stack). [Stated §15.3] |
| Supervisory control | Mission planning, contingency management (e.g., sensor or actuator failure). [Stated §15.3, Ex. 15.1] |
| Online optimization: receding horizon control / MPC | Repeatedly solve a constrained trajectory optimization over a finite horizon, apply the first part, and repeat. It originated in the process industries. [Stated §15.3] |
| Linking continuous and discrete controllers | Combine continuous feedback with discrete decision logic (hybrid systems). [Stated §15.3] |
| Model checking, temporal logic, program synthesis | Formally verify (or automatically synthesize) logic so that it satisfies safety and performance specs, instead of relying only on simulation. [Stated §15.3] |

**Example:** autonomous driving (15.1).

**Prerequisites:** Ch. 1 §1.7–1.8 (logic, layers); Ch. 3 (finite state machines, hybrid systems); Ch. 7 §7.5 (optimization, LQR); Ch. 8 §8.5 (trajectory generation + feedback, 2DOF).

---

### Module D — Bottom-up architectures (§15.4) ★
**Purpose:** Build complex capability by interconnecting standard low-level control structures. [Stated §15.4]

| Structure | Use it when… | One-line gloss |
|---|---|---|
| Cascade control | One actuator, **several sensors** | Inner fast loop + outer slow loop (the inner–outer design of Fig. 1.13 and Ex. 12.9). [Stated §15.4] |
| Mid-range control | **Several actuators** on one output | A fast actuator with small range plus a slow one with wide range (e.g., CD player). [Stated §15.4] |
| Selector control | **Constraints / equipment protection** | Max/min selectors switch between controllers to keep auxiliary variables within limits. [Stated §15.4] |
| Internal model control (IMC) | Disturbance estimation | Use a process model to estimate the effect of disturbances on the output (a disturbance observer). [Stated §15.4] |
| Smith predictor | **Time delays** | A model of the delay-free process gives the controller "phase advance". [Stated §15.4] |
| Complementary filtering | **Sensor fusion** | Combine a slow, accurate sensor with a fast, drifting one (e.g., GPS + inertial). [Stated §15.4] |
| Extremum seeking / self-optimization | **Optimize an objective** whose optimum is unknown | Adjust the reference to maximize or minimize a measured objective. [Stated §15.4] |

**Prerequisites:** Ch. 11 (PID); Ch. 12 (nested loops, feedforward); Ch. 8 (observers, for IMC); Ch. 9–10 (time delays, for the Smith predictor); Ch. 1 §1.7 (selectors).

---

### Module E — Interaction (§15.5)
**Purpose:** Handle the unintended coupling that appears when a system is built loop by loop. [Stated §15.5]

| Concept | One-line gloss |
|---|---|
| Loop interaction | Closing one loop changes the dynamics seen by another. [Stated §15.5] |
| Relative gain array (RGA) | A simple measure of interaction, computed from the static gain matrix and its inverse. [Stated §15.5] |
| Pairing | Use the RGA to decide which input should control which output. [Stated §15.5, Ex. 15.2–15.3] |
| Parallel systems | Several subsystems produce the same effect (e.g., motors on each wheel, synchronized generators), so they must share the load properly. [Stated §15.5, Ex. 15.4] |

**Prerequisites:** Ch. 9 (transfer function matrices, zero frequency gain); matrix inverse; Ch. 7–8 (reachability and observability, mentioned as the conceptual root of pairing). [Stated §15.5: pairing as "a prototype for a system that lacks reachability and observability"]

---

### Module F — Adaptation and learning (§15.6)
**Purpose:** Brief overview of controllers that adapt, learn, and reason. [Stated §15.6]

| Concept | One-line gloss |
|---|---|
| Adaptation vs learning vs reasoning | Adjust to a situation / acquire knowledge from experience / infer from facts or logic. [Stated §15.6] |
| Gain scheduling | Precomputed gains changed with the operating condition (see also §8.5). [Stated §15.6] |
| Model reference adaptive control (MRAC) | Adjust parameters so the closed loop matches a reference model. [Stated §15.6] |
| Self-tuning regulators | Estimate a process model online, then redesign the controller. [Stated §15.6] |
| Auto-tuning | Automatic tuning on demand (e.g., relay auto-tuning from §11.3). [Stated §15.6 / §15.2] |
| Learning (supervised, reinforcement) | Learn a nonlinear function from examples or from rewards. [Stated §15.6] |
| BOXES method | Early learning controller for pole balancing (a gridded state table). [Stated §15.6, Ex. 15.6] |
| Neural networks | Efficient function approximators that replace huge tables. [Stated §15.6] |
| Deep learning | Many-layer networks, useful for preprocessing information-rich sensors (cameras, spectrometers) in control. [Stated §15.6] |

**Example:** adaptive ship steering (15.5).

**Prerequisites:** Ch. 11 (PID, auto-tuning); Ch. 8 (gain scheduling, estimation); Ch. 13 (limits of robust design, which motivate adaptation).

---

### Module G — Control design in common application fields (§15.7)
**Purpose:** Show the commercial landscape: systems, controllers, and users. [Stated §15.7]

| Field | Book's characterization |
|---|---|
| Aerospace | High-performance systems, highly skilled users. [Stated §15.7] |
| Automotive | Complex systems used by ordinary people. [Stated §15.7] |
| Process industry | Complex systems with many different users (oil refining, pulp and paper, chemicals, power plants). [Stated §15.7] |
| Telecommunication | Billions of systems; the field where Black, Bode, Nyquist, and Shannon laid foundations. [Stated §15.7] |

**Prerequisites:** none beyond the earlier chapters. It is a reading module.

---

## 3. Dependency Map

```
Ch. 1 §1.7–1.8 (logic, layers)   Ch. 3 (FSM/hybrid)   Ch. 7–8 (LQR, observers, 2DOF, gain sched.)
Ch. 9–12 (TFs, delays, PID, nested loops, feedforward)   Ch. 13–14 (robustness, limits)
                 └───────────────────────┬───────────────────────────┘
                                         ▼
Module A: Elements of every control system (sensors, actuators, comms, computers, operators)
                                         │
                                         ▼
Module B: DESIGN PROCESS (design V → model-based design → commissioning)
                                         │
              ┌──────────────────────────┼──────────────────────────┐
              ▼                          ▼                          ▼
Module C: TOP-DOWN               Module D: BOTTOM-UP          Module F: ADAPTATION & LEARNING
layers → supervisory logic       cascade, mid-range,          gain scheduling → MRAC,
→ trajectory gen / MPC           selector, IMC, Smith,        self-tuning → supervised/RL
→ continuous+discrete            complementary filter,        → neural nets → deep learning
→ model checking                 extremum seeking
                                         │
                                         ▼
                                 Module E: INTERACTION (RGA, pairing, parallel systems)

Module G: Application fields (independent reading)
```

**Must be understood first:** the design V / model-based design idea (B) and the layered architecture idea (C). Together they frame everything else.

**Can be learned independently:**
- Each bottom-up structure in Module D can be learned on its own, but each needs its own earlier chapter (see the table).
- Module F (adaptation and learning) does not depend on D or E.
- Module G is pure context.

**Depend on several earlier concepts:**
- MPC (C) needs trajectory generation (Ch. 8 §8.5), optimization (Ch. 7 §7.5), **and** constraints / actuator limits (Ch. 11, 14).
- The Smith predictor (D) needs time delays (Ch. 9), the delay limits (Ch. 14), **and** an internal model (IMC, D).
- Complementary filtering (D) needs frequency-domain thinking (*S + T = 1*-style splitting, Ch. 12) **and** the sensor characteristics. [Inferred link to *S + T = 1*]
- RGA (E) needs the static gain matrix (Ch. 9) **and** matrix inversion.
- Self-tuning regulators (F) need online estimation (Ch. 8) **and** controller design (Ch. 7, 11).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Design V and model-based design (verification vs validation; find faults early); layered architecture (supervisory logic → trajectory generation → feedback); cascade control; selector control for protection; gain scheduling |
| **Supporting** | Receding horizon control / MPC (concept); linking continuous and discrete control; complementary filtering; Smith predictor; IMC; RGA and pairing; adaptation vs learning; supervised vs reinforcement learning |
| **Advanced / postpone** | Model checking and temporal logic; program synthesis; mid-range control; extremum seeking; parallel systems; MRAC and self-tuning details; BOXES; deep learning in control; the application-field survey |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **The whole book.** This chapter is a synthesis and a survey; most structures point back to a specific earlier chapter.
- **Optimization** (constrained optimization, for MPC).
- **Discrete logic / automata** (for linking continuous and discrete control and model checking).
- **Basic machine learning vocabulary** (supervised learning, reinforcement learning, neural networks) for §15.6.

### Terms that need clarification
| Term | Issue |
|---|---|
| Verification vs validation | Often confused: verification = meets the spec; validation = the spec meets the real need. |
| Supervisory control | Here: the higher-level logic layer (modes, contingencies), not the low-level loops. |
| Receding horizon control vs MPC | The book treats them as the same idea: optimize over a horizon, apply the first step, repeat. |
| Cascade vs inner–outer loop | The same structure under different names (§1.5, §12.6, §15.4). |
| IMC / inferential control / disturbance observer | Related names for model-based disturbance estimation. |
| Adaptive vs robust control | Robust: one fixed controller tolerates variation. Adaptive: the controller changes as the variation is learned. §13.4's "limits of robust design" motivates adaptation. |

### Topics the chapter does not explain enough
- **This chapter is a survey.** It "briefly introduc[es]" concepts. [Stated, Preface 2nd ed.] MPC, adaptive control, formal verification, and learning-based control each need dedicated texts (see §15.8).
- **This is the electronic-edition draft.** At least one citation is empty in the source (§15.8: "Complementary filtering is well described in []"). Treat details with care and cross-check them.
- **Safety architecture** (hardware interlocks, E-stops, fault detection) is mentioned (faults, contingency management, selectors) but not systematized.
- **Human factors / operator interfaces** are named as a common element but not developed.
- **How to choose between top-down and bottom-up** for a given project is left to judgment.

### Note for Drillbotics [Inferred]
This chapter is the **closest match to the team plan as a whole**:
- **Design V / model-based design (Module B)** = "sim → toy drill → lab rig, same controller everywhere". Simulation is the virtual hardware for early verification. Use the V to structure the Phase I report.
- **Layered architecture (Module C)** = autonomous mode switching (Stateflow, supervisory layer) → trajectory plan / steering (trajectory generation) → WOB/RPM loops (feedback).
- **Selector control (Module D)** = keeping WOB, torque, and RPM within limits for equipment protection. The plan puts *hard* safety in hardware; selectors are the *soft* software layer above it.
- **Complementary filtering (Module D)** = fusing noisy magnetometer and accelerometer data for survey and toolface estimation.
- **Extremum seeking (Module D)** = a classic way to drive toward the minimum **MSE** (mechanical specific energy) operating point.
- **Adaptation and learning (Module F)** is the conceptual home for the optional ML / Edge-AI bonus.
