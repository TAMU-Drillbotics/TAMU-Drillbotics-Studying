# Chapter 1: An Overview and Brief History of Feedback Control — Layer 1 Knowledge Map

> Source: Franklin, Powell & Emami-Naeini, *Feedback Control of Dynamic Systems*, 7th ed., Chapter 1, pp. 1–22.
> Layer: 1 (knowledge map). What to learn and in what order, not full explanations.
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It introduces the **central idea of feedback control**: "a dynamic system's output can be measured and fed back to a controller of some kind and used to affect the system." [Stated, Perspective] It does this with two everyday examples: a **house furnace with a thermostat** (§1.1) and a **car cruise control** (§1.2).

### What problems does it address?
- What are the parts of a feedback system, and what does each one do?
- Why use feedback instead of simply computing the right input in advance (open loop)?
- What must every control design achieve?
- Why can't we just turn the feedback gain up as high as possible?

### Major components
1. Vocabulary and classifications of control (Perspective, Summary)
2. The components of a feedback system (§1.1)
3. A first quantitative comparison of open-loop and closed-loop control (§1.2)
4. The four goals of feedback design (§1.3)
5. History and a road map of the book (§1.4, §1.5)

### After this chapter you should be able to:
- Name the components of a feedback loop (process, actuator, plant, sensor, controller, input filter) for any example.
- Draw a component block diagram and label the reference, error, control signal, disturbance, and sensor noise.
- Reproduce the cruise-control calculation: open-loop error = 5 mph per 1% grade; closed-loop error reduced by a factor of 101.
- State the four goals (stability, tracking, disturbance rejection, robustness).
- Explain the "gain dilemma": high loop gain reduces error, but too much gain with dynamics causes instability.

---

## 2. Knowledge Structure

### Module A — Vocabulary and classification (Perspective, Summary)
**Purpose:** Give names to the basic kinds of control systems.

| Concept | One-line gloss |
|---|---|
| Control | Making a system variable adhere to a particular value, the **reference value**. [Stated, Summary] |
| Manual vs automatic control | A person in the loop (driving a car) vs machines only (thermostat). [Stated, Perspective] |
| Regulator | A system designed to hold an output steady against unknown disturbances. [Stated, Perspective] |
| Tracking (servo) system | A system designed to follow a changing reference signal. [Stated, Perspective] |
| Open-loop control | The controller does **not** use a measurement of the output. [Stated, Perspective] |
| Closed-loop (feedback) control | The output is measured and fed back to compute the control. [Stated, Perspective] |
| Feedforward | Using advance information about a coming reference or disturbance (e.g., a power plant's known daily demand cycle) as an "early warning" to the controller. [Stated, Perspective] |
| Predictive control | Using observations of the future (looking down the road while driving). Simple feedback is like "driving in a thick fog." [Stated, Perspective] |
| Classical vs modern control | Classical methods use Laplace (or z) transforms; modern methods use ODEs in state form (from the 1960s). The book teaches both. [Stated, Summary] |

**Why together:** These are labels you need before you can describe any specific system.

**Prerequisites:** None.

---

### Module B — Components of a feedback system (§1.1) ★
**Purpose:** Identify the generic building blocks, using the furnace and thermostat (Fig. 1.1) and the generic diagram (Fig. 1.2).

Learn these in order:

| # | Concept | One-line gloss |
|---|---|---|
| 1 | Process | The thing whose output we want to control (the house; output = room temperature). [Stated §1.1] |
| 2 | Disturbance | An outside input that upsets the output (heat loss through the walls, wind, open doors). [Stated §1.1] |
| 3 | Actuator | The device that can change the process output (the gas furnace). Power, speed, and reliability usually matter more than accuracy. [Stated §1.1] |
| 4 | Plant | Process + actuator together. [Stated §1.1] |
| 5 | Sensor | Measures the output (the thermostat's thermometer). Its output always contains **sensor noise**. [Stated §1.1] |
| 6 | Reference (set point) | The desired value of the output. [Stated §1.1] |
| 7 | Input filter | Converts the reference into a usable signal; can also shape it to improve response. [Stated §1.1] |
| 8 | Controller | Computes the error (reference − sensor output) and the control signal sent to the actuator. [Stated §1.1] |
| 9 | Component block diagram | A picture of the parts and the direction of information flow. [Stated §1.1] |

**Key observations [Stated §1.1]:**
- **Sensor placement matters:** the sensed variable may differ from the true controlled variable (one thermostat in one room vs the whole house).
- **Process design matters:** a well-insulated house is easier to control. "The earlier the issues of control are introduced into the process design, the better."
- **Loops inside loops:** many feedback systems contain components that are themselves feedback systems.

**Why together:** Every feedback system, from a furnace to a drill rig, has these same roles.

**Prerequisites:** Module A (open vs closed loop).

---

### Module C — A first analysis of feedback: cruise control (§1.2)
**Purpose:** Show *with numbers* why feedback helps, using a **static** model (dynamics are ignored on purpose).

| Concept | One-line gloss |
|---|---|
| Static linear model | Speed = 10 × (throttle angle) − 5 × (% road grade), measured near 65 mph. [Stated §1.2] |
| Block diagram as math | Lines carry signals; a block multiplies its input by a gain; a summer (Σ) adds or subtracts signals. [Stated §1.2] |
| Open-loop result | *y* = *r* − 5*w*: a 1% grade gives a 5 mph (7.69%) error. Zero error on level road only if the controller gain is the **exact inverse** of the plant gain. [Stated §1.2] |
| Closed-loop result | With controller gain 10: *y* = (100/101)*r* − (5/101)*w*. Sensitivity to grade is reduced by a factor of **101**, at the cost of a small error on level ground. [Stated §1.2] |
| Loop gain | The product of plant gain and controller gain (here 100). Errors are small when the loop gain is large. [Stated §1.2] |
| The gain dilemma | With dynamics, too much gain makes the response worse or unstable (a public-address system "squeals"). Getting the gain high without instability "is what much of feedback control design is all about." [Stated §1.2] |

**Why together:** This is the whole argument for feedback in miniature: large loop gain → small errors and low sensitivity; but gain is limited by stability.

**Prerequisites:** Module B; basic algebra (solving one linear equation).

**Relationship [Stated §1.2]:** Open-loop error caused by a plant-gain error equals that gain error in percent; feedback divides it by roughly the loop gain. Ch. 4 makes this exact with the **sensitivity function**.

---

### Module D — Feedback system fundamentals: the four goals (§1.3) ★
**Purpose:** State what any control design must achieve.

| Goal | One-line gloss |
|---|---|
| Stability | "The system must be stable at all times. This is an absolute requirement." [Stated §1.3] |
| Tracking | The output follows the command reference as closely as possible (lane keeping, autopilot landing). [Stated §1.3] |
| Disturbance rejection | The output is as insensitive as possible to disturbances (thermostat vs outside temperature). [Stated §1.3] |
| Robustness | The goals above are still met if the design model is inaccurate or the plant changes over time. [Stated §1.3] |

**Two causes of instability [Stated §1.3]:** (1) the plant itself is unstable (a Segway falls over without control); (2) the feedback itself makes things worse, a "vicious circle."

**Other points [Stated §1.3]:** Design needs a mathematical model, but real systems are complex and nonlinear, so we design on a simplified model and need robustness. Computers serve both as design aids and as embedded controllers.

**Why together:** These four goals are the yardsticks for every method in Ch. 4–10.

**Prerequisites:** Module C (the gain dilemma motivates "stability" and "robustness").

---

### Module E — History and road map (§1.4, §1.5)
**Purpose:** Give context and preview the book.

| Concept | One-line gloss |
|---|---|
| Early regulators | Water clocks and wine vessels with float valves (sensor and actuator in one device), Drebbel's incubator (~1620), Watt's fly-ball governor. [Stated §1.4] |
| Path to theory | Stability analysis of governors (Maxwell), Routh's stability test, feedback amplifiers, frequency-response tools and Nyquist's criterion, root locus, state-space and optimal methods. [Stated §1.4 and the book's inside-cover timeline] |
| Book road map | Ch. 2 models → Ch. 3 response and stability → Ch. 4 feedback basics and PID → Ch. 5/6/7 three design methods → Ch. 8 digital → Ch. 9 nonlinear → Ch. 10 case studies. [Stated §1.5] |
| Scope | SISO linear design for linear plant models; multivariable control and detailed hardware testing are outside the book. [Stated §1.5] |
| Software | Matlab and Simulink are used throughout; figure files are on www.FPE7e.com. [Stated §1.5] |

**Why together:** Orientation material. Nothing later depends on the historical details.

**Prerequisites:** None.

---

## 3. Dependency Map

```
Module A: Vocabulary (open/closed loop, regulator/servo, feedforward)
      │
      ▼
Module B: Components ★  process → disturbance → actuator → plant
                         sensor (+ noise) → reference → input filter → controller → error
      │
      ▼
Module C: Cruise-control numbers (static)
      open-loop error  vs  closed-loop error  →  loop gain  →  gain dilemma
      │
      ▼
Module D: Four goals ★  stability · tracking · disturbance rejection · robustness

Module E: History & road map   (independent; read any time)
```

**Must be understood first:** open loop vs closed loop; the roles of plant, sensor, actuator, controller; the error signal.

**Can be learned independently:**
- Module E (history) does not feed any later concept.
- Regulator vs tracking (Module A) and the component list (Module B) can be learned in either order.

**Depend on several earlier concepts:**
- The **gain dilemma** (Module C) needs *loop gain* (C) **and** the idea that real systems have *dynamics* (stated here; developed in Ch. 3).
- **Robustness** (Module D) needs *model* (D) **and** the open-loop sensitivity to plant-gain errors (C).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Open-loop vs closed-loop control; process, actuator, plant, sensor, controller, reference, error, disturbance, sensor noise; component block diagram; loop gain; the four goals; the gain dilemma |
| **Supporting** | Regulator vs servo; feedforward (idea only); sensor placement; cruise-control calculation; two causes of instability; classical vs modern methods |
| **Advanced / postpone** | Predictive control; history details (§1.4); book road map details |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Basic algebra:** solving simple linear equations (for the cruise-control example).
- **Physical intuition** about heat, speed, and forces. No calculus is needed yet.

### Terms that need clarification
| Term | Issue |
|---|---|
| "Plant" vs "process" | The process is the thing being controlled. The plant is process **plus actuator**. Many people use the two words loosely. |
| "Gain" | Here it means a simple multiplier (output/input ratio of a block). Later it becomes frequency-dependent (Ch. 3, 6). |
| "Stable" | Used informally here. The precise definition (all poles in the left half-plane) comes in §3.6. |
| "Error" | Reference − measured output. Note that it uses the **measured** output, so sensor noise enters the error. |
| "Feedforward" | Only named here. Ch. 4 §4.4 gives one concrete method (plant model inversion). |

### Topics the chapter does not explain enough
- **Dynamics are deliberately ignored** in §1.2. The chapter asserts that high gain can cause instability but cannot show why until Ch. 3–4.
- **Sensor noise** is named but not analyzed; its trade-off with disturbance rejection is in Ch. 4 §4.1.3.
- **Robustness** is stated as a goal with no way to measure it yet (see Ch. 4 §4.1.4 sensitivity, Ch. 6 §6.4 margins and §6.7.7).

### Note for Drillbotics [Inferred]
- Practice Module B on the rig: for the **weight-on-bit (WOB)** loop and the **RPM** loop, name the process, actuator, sensor, disturbance (formation changes, friction), and sensor noise.
- Most drilling loops are **regulators** (hold WOB or RPM at a set point). Following a planned trajectory is a **tracking** problem.
- Module B's "loops inside loops" matches a likely architecture: inner motor-speed loops inside an outer drilling-parameter loop.
