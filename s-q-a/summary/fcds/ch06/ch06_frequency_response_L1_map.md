# Chapter 6: The Frequency-Response Design Method — Layer 1 Knowledge Map

> Source: Franklin, Powell & Emami-Naeini, *Feedback Control of Dynamic Systems*, 7th ed., Chapter 6, pp. 308–432.
> Layer: 1 (knowledge map). What to learn and in what order, not full explanations.
>
> **Tags:** **[Stated §x.y]** = said directly in the textbook. **[Inferred]** = my reading; verify it.

---

## 1. Domain Overview

### What does this chapter study?
It studies **how to analyze and design feedback loops using the frequency response** — how a system scales and shifts sinusoids of each frequency — mainly through **Bode plots** and **Nyquist plots** of the open-loop transfer function.

### What problems does it address? [Stated, Perspective]
- Design that still works when the plant model is uncertain, e.g., poorly known high-frequency resonances. "Frequency-response design is popular primarily because it provides good designs in the face of uncertainty in the plant model."
- Design directly from **measured** data: amplitude and phase measurements from a sinusoidal test are enough, with no pole/zero model needed.
- Meeting specifications that are themselves often written in frequency terms.
- Determining closed-loop stability (Nyquist) and **how close** a loop is to instability (gain and phase margins).

The book adds that this is "probably" the most common design method in industry. [Stated, Perspective]

### Major components
1. Frequency response and Bode plot techniques (§6.1) ★
2. Neutral stability and the Nyquist stability criterion (§6.2, §6.3)
3. Stability margins (§6.4) ★
4. Bode's gain–phase relationship (§6.5)
5. Closed-loop frequency response (§6.6)
6. Compensation: PD, lead, PI, lag, PID; design considerations (§6.7) ★
7. Sensitivity-function specifications and limits (§6.7.7–6.7.8, optional)
8. Time delay (§6.8) and Nichols chart (§6.9) (optional)

### After this chapter you should be able to:
- Sketch the asymptotic Bode magnitude and phase of a transfer function by hand, and check it with `bode`.
- Read gain margin (GM), phase margin (PM), and crossover frequency from a Bode or Nyquist plot.
- Use the Nyquist criterion *Z* = *N* + *P* to decide closed-loop stability.
- Relate PM to damping (ζ ≈ PM/100) and crossover frequency to bandwidth and speed.
- Design a lead or lag compensator to meet PM and steady-state error specs.
- State the low-frequency and high-frequency requirements on the loop gain (error/disturbance vs noise/unmodeled dynamics).

---

## 2. Knowledge Structure

### Module A — Frequency response and Bode plots (§6.1) ★
**Purpose:** Represent a system by its gain and phase at each frequency, and plot it quickly.

Learn these in order:

| # | Concept | One-line gloss |
|---|---|---|
| 1 | Frequency response | For a stable *G*(*s*) driven by *A* sin ω*t*, the steady output is *AM* sin(ω*t* + φ) with *M* = \|*G*(*j*ω)\|, φ = ∠*G*(*j*ω). [Stated §6.1] |
| 2 | Bandwidth ω_BW | The highest frequency at which the closed-loop output still tracks the input well; defined by the closed-loop magnitude falling to 0.707 (−3 dB). A measure of speed of response. [Stated §6.1, Summary] |
| 3 | Resonant peak *M*ᵣ | The peak of the closed-loop magnitude; related to damping. For a second-order system the peak is ≈ 1/(2ζ). [Stated §6.1, Summary] |
| 4 | Bode plot | Magnitude on a log scale (often in decibels) and phase on a linear scale, both vs log frequency. Products become sums. [Stated §6.1.1, Summary] |
| 5 | Bode form | Write *G* as *K*₀(*j*ω)ⁿ(*j*ωτ₁ + 1)…/((*j*ωτₐ + 1)…) so each factor is plotted separately. [Stated §6.1.1] |
| 6 | Three classes of terms | Class 1: *K*₀(*j*ω)ⁿ (pole/zero at origin); Class 2: first-order (*j*ωτ + 1); Class 3: second-order (complex) terms. [Stated §6.1.1] |
| 7 | Break points and asymptotes | At ω = 1/τ the magnitude slope changes by ±1 (±20 dB/decade) for first-order, ±2 for second-order terms. [Stated §6.1.1] |
| 8 | Nonminimum-phase systems | An RHP zero gives the same magnitude as an LHP zero but **more phase lag**. [Stated §6.1.1] |
| 9 | Steady-state errors from Bode | The low-frequency asymptote gives the error constant (e.g., *K*ᵥ for Type 1). [Stated §6.1.2] |

**Why together:** All are about drawing and reading the frequency response.

**Prerequisites:** Ch. 3 §3.1.2 (frequency response); complex numbers; logarithms; Ch. 4 (system type).

---

### Module B — Stability from the frequency response (§6.2, §6.3)
**Purpose:** Decide closed-loop stability from the open-loop frequency response.

| Concept | One-line gloss |
|---|---|
| Neutral stability | Where the root locus crosses the *j*ω axis: \|*KG*(*j*ω)\| = 1 and ∠*G*(*j*ω) = −180°. Links Ch. 5 to this chapter. [Stated §6.2] |
| Nyquist plot | The plot of *KG*(*s*) in the complex plane as *s* travels around the whole RHP. [Stated §6.3] |
| Argument principle | The number of times a mapped contour encircles the origin = (zeros − poles) inside the contour. The theory behind Nyquist. [Stated §6.3.1] |
| Nyquist criterion | *Z* = *N* + *P*: closed-loop RHP poles = clockwise encirclements of −1 + open-loop RHP poles. Stable iff *Z* = 0. [Stated, Summary] |
| −1/*K* trick | Plot *G* once and count encirclements of −1/*K*, so many gains are checked at once (Review Q 6.14). [Stated, Summary] |

**Prerequisites:** Module A; Ch. 3 stability; Ch. 5 (neutral stability via the root locus). Complex-variable theory helps for the argument principle (web Appendix WA).

**Note [Stated, Perspective]:** "the methodology of frequency-response design does not require that the designer remembers the details of the theory."

---

### Module C — Stability margins (§6.4) ★
**Purpose:** Measure **how far** a stable loop is from instability.

| Concept | One-line gloss |
|---|---|
| Gain crossover frequency ω꜀ | Where \|*KG*(*j*ω)\| = 1 (0 dB). [Stated §6.4, Review Q 6.4] |
| Phase crossover | Where ∠*G*(*j*ω) = −180°. [Stated, Review Q 6.5] |
| Gain margin (GM) | How much the gain can increase before instability (measured at phase crossover). [Stated §6.4] |
| Phase margin (PM) | How much extra phase lag at ω꜀ would cause instability. [Stated §6.4] |
| PM ↔ damping | For a standard second-order system, ζ ≈ PM/100 (PM in degrees). [Stated, Summary] |
| Crossover ↔ bandwidth | ω_BW is approximately ω꜀. [Stated, Summary] |
| Vector margin | One number: the closest distance from the Nyquist plot to the critical point. [Stated §6.4, Summary] |
| Conditionally stable system | Visible on a Bode plot when phase crosses −180° at frequencies where the gain is above 1 (Review Q 6.15). [Stated, Review Questions] |

**Prerequisites:** Module A (Bode); Module B (why −1 is the critical point).

---

### Module D — Bode's gain–phase relationship (§6.5)
**Purpose:** A shortcut that turns stability into a slope rule.

| Concept | One-line gloss |
|---|---|
| Gain–phase relation | For a stable **minimum-phase** system, phase is set by the magnitude slope: ∠*G* ≈ *n* × 90°, where *n* is the slope (decades of magnitude per decade of frequency). [Stated §6.5, Summary] |
| Design rule | "Shape the frequency-response magnitude so that it crosses magnitude 1 with a slope of −1" for a good PM. [Stated, Chapter Overview] |

**Prerequisites:** Module A (slopes); Module C (PM); Module A #8 (minimum vs nonminimum phase).

---

### Module E — Closed-loop frequency response (§6.6)
**Purpose:** Connect the open-loop Bode plot to the closed-loop response *T* = *GD*꜀/(1 + *GD*꜀). [Stated §6.6, Summary]

**Prerequisites:** Module C; Ch. 4 (*T*).

---

### Module F — Compensation (§6.7) ★
**Purpose:** Reshape the open-loop Bode plot to meet specs.

| Concept | One-line gloss |
|---|---|
| PD compensation | Adds phase lead (and gain) at high frequency; amplifies noise. [Stated §6.7.1] |
| Lead compensation | (*T*_D*s* + 1)/(α*T*_D*s* + 1), α < 1: a high-pass filter approximating PD. Used when more damping (PM) is needed; tends to raise speed. Maximum phase lead at the geometric mean of its zero and pole; place it at crossover. Step-by-step design procedure given. [Stated §6.7.2, Summary] |
| PI compensation | Raises low-frequency gain (integral action) to reduce steady-state error; keep its break frequency well below crossover. [Stated §6.7.3] |
| Lag compensation | α(*T*ᵢ*s* + 1)/(α*T*ᵢ*s* + 1), α > 1: a low-pass filter approximating PI. Raises low-frequency gain for better steady-state error at fixed bandwidth. Design procedure given. [Stated §6.7.4, Summary] |
| PID compensation | A combination of lead and lag. [Stated §6.7.5, Summary] |
| Design considerations | **Low frequencies:** high loop gain for small tracking error and disturbance rejection. **High frequencies:** low loop gain to reject sensor noise and to stay robust to unmodeled resonances. [Stated §6.7.6, Summary] |
| Gain stabilization vs phase stabilization | Keep an uncertain high-frequency resonance below magnitude 1 (gain stabilization), or design with known phase to handle it (phase stabilization). [Stated §6.7.6] |

**Why together:** Each compensator is a tool to move the crossover, slope, PM, or low-frequency gain.

**Prerequisites:** Modules A–D; Ch. 4 (system type, PID); Ch. 5 (lead/lag in the s-plane).

---

### Module G — Sensitivity-function specs and limits (§6.7.7–6.7.8, optional ▲)
**Purpose:** Express performance and robustness directly with *S* and *T* across frequency.

| Concept | One-line gloss |
|---|---|
| Specs on \|*S*(*j*ω)\| | Tracking and disturbance rejection require \|*S*\| small over a low-frequency band. [Stated §6.7.7] |
| Stability robustness | The loop must stay stable despite model error; this bounds \|*T*\| where the model is uncertain (high frequency). [Stated §6.7.7] |
| Complementary sensitivity *T* | Used for the robustness and noise specs. [Stated §6.7.7] |
| Bode's sensitivity integral | The integral of ln\|*S*\| over frequency is zero (no RHP poles) or positive (with RHP poles). [Stated §6.7.8] |
| Waterbed effect | Pushing \|*S*\| down in one band pushes it up somewhere else. [Stated §6.7.8] |
| Vector margin ↔ peak of \|*S*\| | A large sensitivity peak means a small vector margin. [Stated §6.7.8] |

**Prerequisites:** Ch. 4 (*S*, *T*, *S* + *T* = 1); Module C (vector margin); Module F (design considerations).

---

### Module H — Time delay and alternative plots (§6.8, §6.9, optional ▲)
| Concept | One-line gloss |
|---|---|
| Time delay *e*^{−*sT*_d} | Magnitude 1, phase −ω*T*_d: pure phase lag that grows with frequency, which reduces PM. Handled **exactly** on Bode/Nyquist plots (no Padé approximation needed). [Stated §6.8, Summary] |
| Padé approximation | A rational approximation of a delay, needed for root locus or state space. [Stated §6.8] |
| Nichols chart | Gain vs phase, parameterized by frequency, with closed-loop contours. [Stated §6.9.1, Summary] |
| Inverse Nyquist | Web only. [Stated §6.9.2] |

**Prerequisites:** Modules A–C.

---

## 3. Dependency Map

```
Ch.3: frequency response H(jω), poles/zeros      Ch.4: S, T, system type, PID
Ch.5: root locus, lead/lag (s-plane)
            │
            ▼
Module A ★: Bode plots (Bode form, 3 classes, break points), bandwidth, Mr,
            nonminimum phase, errors from low-freq asymptote
            │
            ├──────────────────────┐
            ▼                      ▼
Module B: neutral stability     Module D: gain–phase relation
          → argument principle             (minimum phase; slope −1 rule)
          → Nyquist Z = N + P              │
            │                              │
            ▼                              │
Module C ★: GM, PM, ωc, PM≈100ζ,  ◄────────┘
            vector margin
            │
            ├──────────────┐
            ▼              ▼
Module E: closed-loop   Module F ★: PD → lead → PI → lag → PID
freq. response                     → design considerations (low/high freq.)
                                    │
                                    ▼
                         Module G (▲): |S| specs, robustness, Bode integral, waterbed
Module H (▲): time delay, Nichols chart   (needs A–C only)
```

**Must be understood first:** *G*(*j*ω) as gain and phase; the Bode plot; gain and phase margins.

**Can be learned independently:**
- The Nyquist criterion (B) can be postponed if you only work with stable, minimum-phase open-loop systems: then PM and GM read from the Bode plot are enough. [Inferred; the book requires Nyquist when *P* ≠ 0 (Review Q 6.13)]
- The gain–phase relation (D) and the closed-loop response (E) do not depend on each other.
- Time delay and Nichols chart (H) do not need compensation (F).

**Depend on several earlier concepts:**
- Lead design (F) needs Bode sketching (A) **and** PM (C) **and** the slope rule (D).
- Sensitivity limits (G) need *S*/*T* (Ch. 4) **and** Bode plots (A) **and** Nyquist/vector margin (B, C).
- PM ≈ 100ζ (C) needs the second-order model (Ch. 3) **and** the PM definition.

---

## 4. Learning Priorities

| Priority | Concepts |
|---|---|
| **Essential** | Frequency response *M*, φ; decibels; Bode form; asymptotic Bode sketching; bandwidth; gain crossover; GM and PM; PM ≈ 100ζ; ω꜀ ≈ ω_BW; crossover with slope −1; lead and lag compensation; low/high-frequency design considerations |
| **Supporting** | Resonant peak; nonminimum-phase phase lag; Nyquist criterion *Z* = *N* + *P*; vector margin; closed-loop frequency response; PI and PID compensation; gain vs phase stabilization; time delay on a Bode plot; reading *K*ᵥ from Bode |
| **Advanced / postpone** | Argument principle proof; −1/*K* encirclement method; sensitivity-function specs; Bode sensitivity integral and waterbed effect; Nichols chart; inverse Nyquist (web); history (§6.10) |

---

## 5. Knowledge Gaps and Assumptions

### Prerequisite knowledge you may need
- **Logarithms and decibels:** dB = 20 log₁₀ \|*G*\|; a decade = a factor of 10 in frequency.
- **Complex numbers:** magnitude and angle of *G*(*j*ω); angle of a product = sum of angles.
- **Complex-variable theory** (contour mapping) for the argument principle; on the web (Appendix WA).
- Ch. 4: *S*, *T*, and system type.

### Terms that need clarification
| Term | Issue |
|---|---|
| Open-loop vs closed-loop frequency response | Margins are read from the **open-loop** *KD*꜀*G*; bandwidth is defined on the **closed-loop** *T*. Easy to mix up. |
| Crossover frequency | Here means gain crossover (\|*L*\| = 1). Phase crossover is a different frequency. |
| Minimum phase | All poles and zeros in the LHP. Only then does the gain–phase relation hold. |
| "Slope of −1" | −1 decade of magnitude per decade of frequency (−20 dB/decade). |
| Lead/lag parameter α | Lead uses α < 1, lag uses α > 1 in this book's formulas. Other books invert this. |

### Topics the chapter does not explain enough
- **The argument principle** is stated with limited proof; the book explicitly says designers need not remember the theory.
- **Nyquist plots with poles on the *j*ω axis** (detours around them) need care; practice examples carefully.
- **Robustness** (§6.7.7–6.7.8) is optional and brief compared with FBS Ch. 13–14.
- **Time delay via Nyquist** and the inverse Nyquist diagram are web only.

### Note for Drillbotics [Inferred]
- Frequency-domain design suits an **uncertain drilling plant** (formation, friction, string dynamics change). Design for adequate PM/GM rather than an exact pole location.
- **Time delay** (§6.8) appears from communication, sensor filtering, or sampling (Ch. 8). Its phase lag eats PM; check it on the Bode plot.
- A **sine-sweep test** on the toy rig gives Bode data directly (Perspective: design from measured data).
- **Design considerations (§6.7.6):** keep the loop gain low at high frequency to avoid exciting drill-string vibration modes and amplifying sensor noise.
