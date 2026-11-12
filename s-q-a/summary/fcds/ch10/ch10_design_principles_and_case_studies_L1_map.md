# Chapter 10: Control System Design: Principles and Case Studies — Layer 1 Knowledge Map

> Source: Franklin, Powell & Emami-Naeini, *Feedback Control of Dynamic Systems*, 7th ed., Chapter 10, pp. 703–803.
> Layer: 1 (knowledge map). What to learn and in what order, not full explanations.
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **how to carry out a complete control design on a real problem**, using all the tools of Ch. 3–9 together instead of one method at a time. [Stated, Perspective] It first gives a general **step-by-step design outline** (§10.1), then applies it to case studies.

### What problems does it address?
- Real problems are not "a linear time-invariant transfer function capable of responding to inputs of arbitrary size." [Stated §10.1] How do you get from a messy physical system to a working controller?
- How do you pick sensors and actuators, not just a compensator?
- When should you change the **plant** instead of the controller?
- How do you verify a design before trusting it?

### Major components
1. An outline of control system design: 9 steps (§10.1) ★
2. Case study: satellite attitude control (§10.2)
3. Case study: Boeing 747 lateral (yaw damper) and longitudinal (altitude hold) control (§10.3)
4. Case study: fuel-air ratio in an automotive engine (§10.4)
5. Case study: hard-disk read/write head (§10.5)
6. Case study: rapid thermal processing (RTP) of semiconductor wafers (§10.6)
7. Case study: chemotaxis in *E. coli* (§10.7)

### After this chapter you should be able to:
- Apply the 9-step outline to a new problem and say what each step should produce.
- List the factors for choosing sensors and actuators.
- Explain, with an example, why collocated sensor/actuator systems are easier to control.
- Explain inner-loop / outer-loop (cascade) design.
- Recognize which earlier tool each case study relies on (notch filter, describing function, delay on a Bode plot, inverse nonlinearity, antiwindup, integral control).

---

## 2. Knowledge Structure

### Module A — The design outline (§10.1) ★
**Purpose:** A general procedure and checklist for any control design. [Stated, Perspective]

Learn these in order:

| Step | What it means |
|---|---|
| 1. Understand the process; translate performance requirements into specs | Specs in time (step-response bounds), frequency (open-loop response bounds), or pole–zero (allowed s-plane region) form (Fig. 10.1). "Do not confuse the approximation with reality." [Stated §10.1] |
| 2. Select sensors | Which variables matter and which can be measured; consider sensing disturbances for feedforward. Factors: number and location, technology, functional performance (linearity, bias, accuracy, bandwidth, resolution, range, noise), physical properties, quality, cost. "If you can't observe it, you can't control it." [Stated §10.1, Summary] |
| 3. Select actuators | Must be able to drive the system to meet specs; same selection factors (force, linear range, speed, power, efficiency...). [Stated §10.1] |
| 4. Make a linear model | Small-signal linearization or feedback linearization (Ch. 9). [Stated, Summary] |
| 5. Try a simple PID or lead-lag controller | It may succeed; in any case it exposes the nature of the problem. [Stated, Summary] |
| 6. Evaluate / modify the plant | If changing the plant (e.g., sensor location, structure) helps, go back to Step 1 or 4. [Stated, Summary] |
| 7. Try an optimal design | SRL/LQR or pole placement with an estimator (Ch. 7); may not be robust to parameter changes. [Stated, Summary] |
| 8. Simulate and verify | Use all analysis tools (root locus, frequency response, GM/PM, transients); test parameter changes and the discrete implementation. [Stated, Summary] |
| 9. Build a prototype | "No control design is acceptable until it has been tested" on real hardware. [Stated, Summary] |

**Other principle [Stated §10.1]:** Control engineers should be involved early in the design of the process itself; some modern systems (e.g., high-performance aircraft) "will not work at all without feedback."

**Prerequisites:** Ch. 1 (goals); Ch. 4 (PID); Ch. 5–7 (design methods); Ch. 8 (digital); Ch. 9 (linearization, nonlinear effects).

---

### Module B — Case studies (§10.2–10.7)
**Purpose:** See the outline applied; each study emphasizes particular tools.

| Case study | Main lessons [Stated, Chapter Overview and Summary] | Tools from earlier chapters |
|---|---|---|
| Satellite attitude (§10.2) | Robust design over parameter changes from beginning-of-life to end-of-life (12–15 years); **notch compensation** for a lightly damped resonance; **collocated** sensor/actuator systems are much easier to control than noncollocated ones | Root locus and lead (Ch. 5), notch (§5.4.3), pole placement with estimator (Ch. 7), simulation |
| Boeing 747 (§10.3) | Nonlinear equations linearized about a flight condition (each axis 4th order); **yaw damper** = inner-loop feedback to aid the pilot (outer loop); **altitude hold** combines inner-loop feedback with outer-loop compensation | Linearization (Ch. 9), root locus (Ch. 5), successive loop closure |
| Engine fuel-air ratio (§10.4) | A **highly nonlinear exhaust-oxygen sensor** (almost all its voltage change happens right at the desired ratio) and a **pure time delay**; Bode design with delay; limit cycles predicted with equivalent gain / describing function and confirmed by simulation | Time delay (§6.8), describing functions and equivalent gain (Ch. 9), Bode design (Ch. 6) |
| Hard-disk drive (§10.5) | Position control in an uncertain environment where **bandwidth** is the key performance measure | Frequency response (Ch. 6), digital control (Ch. 8) |
| RTP wafer processing (§10.6) | Temperature tracking and disturbance rejection for a highly nonlinear thermal plant; nonlinear lamp actuator **cancelled with its inverse**; **actuator saturation** and a control that **cannot go negative** | Inverse nonlinearity (§9.2.3), antiwindup (§9.3.1), robust tracking (Ch. 7 §7.10) |
| *E. coli* chemotaxis (§10.7) | Control ideas applied to systems biology; robust perfect adaptation explained as **integral control** of receptor sensitivity | Integral control and system type (Ch. 4) |

**Cross-cutting lesson [Stated, Summary]:** "In all cases the designer needs to be able to use multiple tools, including the root locus, the frequency response, pole placement by state feedback, and simulation of time responses."

**Prerequisites:** Module A; the chapters listed in each row.

**Recommendation [Inferred]:** Study 1–2 case studies in depth (satellite for structure/collocation; RTP or engine for nonlinear realities), and skim the rest.

---

## 3. Dependency Map

```
Ch.1–9 (all tools)
      │
      ▼
Module A ★: 9-step outline
  specs → sensors → actuators → linear model → simple PID/lead-lag
        → evaluate/modify plant ─┐ (loop back to step 1 or 4)
        → optimal design (SRL / pole placement + estimator)
        → simulate & verify → prototype
      │
      ▼
Module B: case studies (independent of each other)
  satellite ── collocation, notch, robustness to parameter change
  747 ──────── linearization, inner/outer loops
  engine ───── time delay, nonlinear sensor, limit cycles
  disk drive ─ bandwidth, uncertainty
  RTP ──────── inverse nonlinearity, saturation, one-sided control
  E. coli ──── integral control in biology
```

**Must be understood first:** the design outline (Module A).

**Can be learned independently:** every case study (Module B) is self-contained; read them in any order.

**Depend on several earlier concepts:**
- The engine case study needs time delay on Bode plots (Ch. 6) **and** describing functions (Ch. 9).
- The RTP case study needs inverse nonlinearity **and** antiwindup (Ch. 9) **and** tracking design (Ch. 4/7).
- The satellite case study needs notch compensation (Ch. 5) **and** collocation (Ch. 2) **and** state-space design (Ch. 7).

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | The 9-step design outline; specs in time/frequency/pole–zero form; sensor and actuator selection factors; "try simple PID first"; modify the plant when it helps; simulate and verify; build a prototype |
| **Supporting** | Collocated vs noncollocated (satellite); inner-loop / outer-loop design (747); time delay + nonlinear sensor (engine); actuator saturation and inverse nonlinearity (RTP) |
| **Advanced / postpone** | Full details of each case study's equations; disk-drive and *E. coli* studies; history of applications (§10.8) |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- All of Ch. 3–9. This chapter adds no new theory.
- Some domain background for each case study (aircraft dynamics, thermal processes, biology); the book gives the models without full derivations.

### Terms that need clarification
| Term | Issue |
|---|---|
| Inner loop / outer loop | A fast inner feedback loop (e.g., pitch-rate feedback) stabilizes or shapes the plant; a slower outer loop controls the main variable (e.g., altitude). Also called cascade control. [Inferred naming] |
| BOL / EOL | Beginning-of-life / end-of-life of a satellite; parameters drift as fuel is used. [Stated, Chapter Overview] |
| Notch filter | A compensator with a "dent" (notch) in its frequency response at a resonance; also called a band-reject filter. [Stated §10.2] |
| "Optimal design" | Here means SRL/LQR or pole placement (Ch. 7), not a general optimization. |

### Topics the chapter does not explain enough
- **Multivariable (MIMO) design** appears only in the RTP case study; the book does not teach MIMO methods. [Stated §1.5]
- **Requirements engineering** (how to get specs from stakeholders) is described in one paragraph (Step 1).
- **Hardware testing and system identification** are recommended (Step 9) but not taught; see web Appendix W3.7 or FBS Ch. 3.
- **Safety, fault handling, and mode logic** are not part of the outline. [Inferred]

### Note for Drillbotics [Inferred]
- **Module A is a ready-made skeleton for the Phase I design report**: specs → sensors → actuators → linear model → PID → plant changes → advanced design → simulation → toy-drill / lab-rig prototype. It matches the team plan's "sim → toy drill → lab rig" path.
- **Step 2/3 (sensors and actuators)** correspond to the plan's "signal & actuator contract" with the hardware team.
- The **RTP** case (saturation, one-sided actuator, inverse nonlinearity) and the **engine** case (time delay, nonlinear sensor) are the closest analogs to drilling-rig realities.
- **Step 6 (modify the plant)** is a reminder to ask the hardware team for better sensor placement before over-engineering the controller.
