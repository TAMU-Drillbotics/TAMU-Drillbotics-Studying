# Drillbotics 2027 — ML Team Plan


## Direction

Build a simple, reliable control system that meets every mandatory requirement, develop it in simulation first, and move the same controller onto real hardware step by step: **sim → *toy drill* → lab rig**. ML, Ansys, and other extras are optional bonuses, started only once the required work is on track.

## Guiding principles

- **Mandatory first.** Required deliverables come before bonus work.
- **Safety lives in hardware.** E-stop, interlocks, and limits are enforced by the hardware layer, so a software crash can never make the rig unsafe.
- **Toolchain.** Use MATLAB/Simulink and Simscape and later Ansys for simulation, control, logging, and display.
- **Same controller everywhere.** The controller code stays the same across sim, toy drill, and lab rig.
- **Logging.** Every run writes a `drilling_timeseries.csv` to record data, for both logging and future machine learning.

## Major tasks

### 1. Foundations
- Learn Simulink, Simscape, and Stateflow (MathWorks Onramps).
- Agree on a **signal & actuator contract** with the hardware team and physics team: sensors, units, rates, interfaces, limits, and who enforces each limit.

### 2. Kinematic simulator & autonomous logic
- Trajectory and survey math: minimum curvature, dogleg severity, distance to plan.
- Simple steering model (toolface and slide length → build/turn), maybe adding noise to become more realistic.
- Autonomous mode switching (drill, survey, slide, rotate) and survey qualification.
- Drill a dummy build–drop–hold trajectory end-to-end in sim.
- Merge with Ansys simulation for more realistic physical simulation. 

### 3. Phase I design report
- Control architecture, sim results, an assumptions list, the data handling plan, and a UI wireframe with the human factors section.

### 4. Physics plant & drilling control
- Simscape motor and drillstring model, using the provided bit model.
- Closed-loop WOB/RPM control with limits; basic MSE and stick-slip handling.

### *5. Learning outcome checker: Toy drill (debatable)*
- *Run the same controller on real I/O.*
- *Record real sensor data (especially magnetometer noise near the motors) and calibrate the sim against it.*
- *Currently considering being replaced by reversed pendulum*

### Ongoing: display (user interface to interact with the rig)
- Build a monitoring and supervisory display alongside the control system. The tech-stack is still open.

### Optional (only if time is available)
- ML / Edge-AI bonus, advanced stick-slip control, and evidence for the innovation multiplier.

## Key milestones

1. Signal contract agreed
2. Dummy trajectory drilled in sim
3. Phase I report
4. *Toy drill running the controller*
5. Lab rig drilling autonomously
6. Final testing → competition

## Open questions

- Available MATLAB toolbox licenses