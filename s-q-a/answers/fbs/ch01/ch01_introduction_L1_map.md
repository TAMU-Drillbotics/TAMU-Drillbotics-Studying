# Chapter 1: Introduction — Layer 1 Knowledge Map

> Source: Åström & Murray, *Feedback Systems*, 2nd ed. (fbs-public_24Jul2020.pdf), Chapter 1, pp. 1-1 to 1-28.
> Layer: 1 (knowledge map). This file says **what** to learn and **in what order**. It does not explain each concept in depth yet; that is Layer 2.
>
> **Tags used in this file**
> - **[Stated §x.y]**: the textbook says this directly, in that section.
> - **[Inferred]**: my own reading of how the ideas connect. The book does not state it. Check it against the text yourself.

---

## 1. Domain Overview

### What does this chapter study?
It studies the **idea of feedback** and the engineering field built on it, called **control**. It is a qualitative chapter with almost no math. Its job is to give you the vocabulary, the motivation, and a preview of the tools in Chapters 2–15. [Stated, chapter intro: "provide the context for current tools in feedback and control"]

### What problems does it address?
- What *is* feedback, and how is it different from simply sending commands (feedforward)?
- Why do engineers and nature both use feedback so much?
- What can go wrong with feedback?
- What do the simplest feedback controllers look like?
- How do simple loops combine into real systems with logic and layers?

### Major components (the modules below)
1. Basic vocabulary: dynamical system, feedback, open and closed loop (§1.1)
2. Feedforward, the counterpart of feedback (§1.2)
3. What "control" means: sense → compute → actuate (§1.3)
4. Where feedback is used: application examples (§1.4)
5. What feedback is good for, and what it costs: feedback properties (§1.5)
6. The simplest feedback laws: on-off, P, PI, PID (§1.6)
7. Feedback plus logic: modes and selectors (§1.7)
8. Control architectures: layers of control (§1.8)

### After this chapter you should be able to:
- Say in your own words what feedback is, and tell a closed loop system from an open loop one.
- Compare feedback with feedforward on four points (Table 1.1).
- Name the four main uses of feedback (robustness, designing dynamics, modularity, and, as a cost, its challenges) and give one example of each.
- Describe on-off, proportional, and PID control at the level of "what does the controller react to".
- Explain why real controllers also need logic (modes) and layers (optimization above regulation).

---

## 2. Knowledge Structure

### Module A — Feedback vocabulary (§1.1)
**Purpose:** Fix the basic words used in the rest of the book.

**Important concepts**
| Concept | One-line gloss (not a full definition) |
|---|---|
| Dynamical system | A system whose behavior changes over time, often in response to outside forcing. [Stated §1.1] |
| Feedback | Two (or more) dynamical systems connected so that each one influences the other. [Stated §1.1] |
| Closed loop system | Systems connected in a cycle (a loop exists). [Stated §1.1] |
| Open loop system | The same systems with the loop cut, so influence flows one way only. [Stated §1.1] |
| Negative feedback | Feedback that reacts to a change so as to *reduce* it. [Stated §1.1] |
| Positive feedback | Feedback where an increase leads to further increase; usually limited by saturation. [Stated §1.1] |
| Circular causality | Because A affects B and B affects A, simple cause → effect reasoning fails. [Stated §1.1] |

**Why these belong together:** They are the minimum language needed to *describe* a feedback system before analyzing it.

**Examples used in the book:** blood glucose regulation (insulin/glucagon), centrifugal (flyball) governor on Watt's steam engine.

**Prerequisites:** none (this is the entry point).

---

### Module B — Feedforward (§1.2)
**Purpose:** Introduce the main alternative to feedback, so that feedback can be understood by contrast.

**Important concepts**
| Concept | One-line gloss |
|---|---|
| Feedforward | Measure a disturbance (or use a known command) *before* it affects the output, and act on it in advance. [Stated §1.2] |
| Reference (command) signal *r* | The desired output. [Stated §1.2] |
| Disturbance *v* | An outside signal that pushes the process away from what we want. [Stated §1.2] |
| Process *P* / Controller *C* | The system being controlled / the system computing the input. [Stated §1.2] |
| Table 1.1 comparison | Feedback: closed loop, acts on deviations, robust to model uncertainty, risk of instability. Feedforward: open loop, acts on plans, sensitive to model uncertainty, no risk of instability. [Stated §1.2, Table 1.1] |

**Why these belong together:** Feedforward only makes sense in comparison with feedback. The table is the core of this module.

**Prerequisites:** Module A (closed and open loop, feedback).

**Key relationship [Stated §1.2]:** Feedback is *reactive*: an error must already exist before it acts. Feedforward is *proactive*, but it needs a good model of the process. The book adds that combining the two is often best.

---

### Module C — What is control? (§1.3)
**Purpose:** Define the engineering discipline and its basic loop.

**Important concepts**
| Concept | One-line gloss |
|---|---|
| Control | "The use of algorithms and feedback in engineered systems." [Stated §1.3] |
| Sense → compute → actuate loop | A controller measures the system, compares it with the desired behavior, computes a correction using a model, and acts. [Stated §1.3] |
| Sensor / actuator | The part that measures / the part that applies the correction. [Inferred: standard terms. §1.3 describes them through the sensing and actuation steps] |
| Control as an information science | Control uses information in analog and digital form; it is mostly implemented in software. [Stated §1.3] |

**Why these belong together:** They define what a "controller" physically is.

**Prerequisites:** Module A.

---

### Module D — Uses of feedback and control (§1.4)
**Purpose:** Motivation. Show how widely feedback is used.

**Important concepts:** The concrete domains: power networks, biology and medicine (voltage clamp for measuring ion currents), economics and supply chains (Forrester's supply chain dynamics), and others.

**Why this is a module:** It holds examples, not new theory. These are *instances* of Modules A–C.

**Prerequisites:** Modules A and C.

**Priority note:** You can skim this for the first pass. It does not block anything later.

---

### Module E — Feedback properties (§1.5) ★ the core of the chapter
**Purpose:** Explain *why* feedback is so useful, and what it costs.

**Important concepts**
| Concept | One-line gloss | Book example |
|---|---|---|
| Robustness to uncertainty | Feedback keeps the output near the target even when disturbances act or process parameters change. [Stated §1.5] | Cruise control on a hill with different car masses (Fig. 1.11); Black's negative feedback amplifier (1927) |
| Design of dynamics | Feedback can change how a system behaves over time: stabilize unstable systems, speed up sluggish ones, hold drifting ones. [Stated §1.5] | Wright Flyer (unstable but maneuverable); Sperry autopilot |
| Creating modularity | Feedback lets a component keep its input/output behavior even if its surroundings change. This allows nested (cascaded) loops. [Stated §1.5] | Motor drive with current → velocity → position loops (Fig. 1.13) |
| Challenges of feedback | Possible **instability**; **injects measurement noise**; **complexity and cost** of embedding control. [Stated §1.5] | Microphone howl (positive feedback); early automotive electronics |

**Why these belong together:** They are the "pros and cons" list of feedback. Every later chapter formalizes one or more of these properties.

**Prerequisites:** Module A (negative feedback, closed loop). Module B helps for contrast.

**Relationships**
- Robustness ⟷ instability risk: **opposition / trade-off**. Both come from the same closed loop. [Stated §1.5 that both exist. Calling it a trade-off here is Inferred; the book formalizes the trade-off in Ch. 12–14]
- Modularity *depends on* robustness: an inner loop can be treated as a fixed block only because feedback makes its closed loop behavior insensitive to changes. [Inferred from §1.5 text on cascaded loops]

---

### Module F — Simple forms of feedback (§1.6)
**Purpose:** Preview the simplest control laws studied formally later (Ch. 2, 11).

**Important concepts (learn them in this order)**
| Concept | One-line gloss |
|---|---|
| Control error *e = r − y* | Desired output minus actual output. [Stated §1.6] |
| On-off control | Apply full action in one direction or the other depending on the sign of *e*; no parameters to tune; tends to oscillate. [Stated §1.6] |
| Dead zone / hysteresis | Changes to on-off control that avoid chattering near *e = 0*. [Stated §1.6, Fig. 1.14] |
| Proportional (P) control | Action proportional to error (gain *k_p*), within a linear range; often leaves a steady offset. [Stated §1.6] |
| Saturation / linear range | The actuator has limits (*u_min, u_max*); the controller is linear only for errors inside a range. [Stated §1.6] |
| Integral (I) action | Action proportional to the *accumulated* error; gives zero steady-state error. [Stated §1.6] |
| Derivative (D) action | Action based on the error's rate of change; "predicts" the error *T_d* ahead. [Stated §1.6] |
| PID controller | P + I + D = present, past, and future of the error. Many industrial controllers are actually PI. [Stated §1.6] |

**Why these belong together:** Each one fixes a weakness of the one before it: on-off → oscillation; P → steady offset; I → removes the offset; D → adds prediction. [Stated as a progression in §1.6]

**Prerequisites:** Module A (feedback), Module C (sensing/actuation).

---

### Module G — Combining feedback with logic (§1.7)
**Purpose:** Show that real controllers also switch between modes, protect equipment, and handle saturation.

**Important concepts**
| Concept | One-line gloss |
|---|---|
| Operating modes | Different control behavior for different situations (e.g., off/standby/cruise/hold). [Stated §1.7] |
| Finite state machine | A model of mode switching: states plus button- or event-triggered transitions. [Stated §1.7, Fig. 1.16] |
| Selector (max/min) | A static block that outputs the largest/smallest of its inputs; used to implement logic in engines and power systems. [Stated §1.7, Fig. 1.18] |

**Examples:** cruise control FSM, server farm cooling, air–fuel ratio control on ship boilers.

**Prerequisites:** Module F (the loops being switched are P/PI/PID loops).

---

### Module H — Control system architectures (§1.8)
**Purpose:** Show that real systems are layered hierarchies, not single loops. Chapter 15 covers this in depth. [Stated §1.8]

**Important concepts**
| Concept | One-line gloss |
|---|---|
| Layered decomposition | Low layer: regulation (fast loops); higher layers: logic, optimization, planning. [Stated §1.8, Fig. 1.19] |
| Trip/route optimization above regulation | The upper layer computes a plan (e.g., a speed profile), and the lower layer tracks it. [Stated §1.8, GE freight train example] |
| Multi-scale operation | Process plants run control across many time and space scales. [Stated §1.8] |

**Examples:** GE Trip Optimizer (freight trains), pulp and paper plant, autonomous driving.

**Prerequisites:** Modules F and G.

---

## 3. Dependency Map

```
Module A: Feedback vocabulary (dynamical system, feedback, open/closed loop, ±feedback)
   │
   ├──► Module B: Feedforward  ──────────────┐
   │       (needs: open vs closed loop)      │ contrast
   ├──► Module C: What is control            │
   │       (sense → compute → actuate)       │
   │        │                                ▼
   │        ├──► Module D: Uses (examples)   Module E: Feedback properties ★
   │        │       [independent; skimmable]   (robustness, dynamics design,
   │        │                                   modularity, challenges)
   │        ▼
   └──► Module F: Simple feedback laws (on-off → P → I → PID)
              │
              ▼
          Module G: Feedback + logic (modes, FSM, selectors)
              │
              ▼
          Module H: Architectures (layers)   ◄── also needs Module E (modularity)
```

**Must be understood first:** Module A, especially *feedback*, *closed vs open loop*, and *negative feedback*.

**Can be learned independently (after A):**
- Module B (feedforward) and Module C (control) do not depend on each other.
- Module D (uses) can be read at any time after A and C. Nothing depends on it.

**Depend on several earlier concepts:**
- Module E needs A (loop), and its comparisons only make sense with B (feedforward) in mind.
- Module H needs F (loops), G (logic), and E (modularity). [Inferred: the book does not draw this dependency explicitly]

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Feedback; closed vs open loop; negative vs positive feedback; feedforward; Table 1.1 comparison; control error *e = r − y*; robustness to uncertainty; design of dynamics; instability as the main risk of feedback; noise injection; P, I, D actions |
| **Supporting** | Sense–compute–actuate loop; modularity via cascaded loops; on-off control with dead zone/hysteresis; saturation and linear range; finite state machine for modes; layered architecture |
| **Advanced / postpone** | Selectors (max/min logic); specific application stories (power grid, supply chain, voltage clamp, GE trip optimizer, paper mill, autonomous driving); the history of feedback |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **None mathematically for this chapter.** Equations (1.1)–(1.4) only use simple algebra and, for PID, the ideas of an **integral** (accumulated area) and a **derivative** (rate of change) from basic calculus.
- A rough idea of a **block diagram** (boxes = systems, arrows = signals) helps. The book uses block diagrams before defining them formally in §2.2 and §3.3.

### Terms that need clarification (used in Ch. 1 before being formally defined)
| Term | Where it is formally defined |
|---|---|
| Stability / instability | Ch. 5 (§5.3) |
| Gain | Informally §2.1; formally Ch. 9 (zero frequency gain) and Ch. 10 |
| Dynamics / "design the dynamics" | Ch. 3 (state models), Ch. 5–7 |
| Steady-state error | Ch. 2 (§2.3–2.4), Ch. 11 |
| Model / model uncertainty | Ch. 3 (§3.1), Ch. 13 |
| Setpoint, reference | Used loosely in Ch. 1; consistent use from Ch. 2 |
| Robustness | Informal here; formal in Ch. 10 (margins) and Ch. 13 |

### Topics the chapter does not explain enough (by design)
- **Why** integral action gives zero steady-state error: §1.6 only says "it can be shown through simple arguments". The argument appears in §2.3–2.4 and Ch. 11.
- **Why** feedback can cause instability: only the microphone example is given. The mechanism (loop gain and phase) is in Ch. 10.
- **How** to choose P/I/D gains: not covered here; see Ch. 2, 11.
- Layered architectures are only previewed; §1.8 itself points to Ch. 15.
- The book's notation changed in the 2nd edition (disturbance = *v*, noise = *w*). Keep this in mind if you also read other textbooks, where *d* and *n* are common. [Stated, Preface to 2nd ed.]

### Note for your Drillbotics work [Inferred: not from the book]
Your team plan describes a **layered system**: mode switching (drill/survey/slide/rotate) is Module G, a trajectory plan above WOB/RPM loops is Module H, and WOB/RPM PID loops are Module F. This chapter is the vocabulary for describing that architecture in the Phase I report.
