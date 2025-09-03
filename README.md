# 2025-Vex-Robotics

End-of-season competition software for our VEX V5 robot.

This repository includes full **device configuration**, multiple **PID controllers** (drive + Lady Brown arm), complete **autonomous strategies** (including winpoint + skills), responsive **driver control**, and a **real-time PID error graph** drawn on the V5 Brain for live tuning.

> Programmers: **Isaiah Thomas & Saavan Kiran** • Team: **Velocity Robotics 20020X** • Environment: **VEXcode Pro V5 (C/C++)**

---

## Key Highlights

- **PID Control System**
  - Drivetrain PID (forward/back + turning) with odometry and inertial feedback
  - Arm PID for Lady Brown with position states and smooth control
  - Speed capping and settle detection logic for reliable moves
  - On-Brain error graph for monitoring + tuning (green trace)

- **Drive & Turn PID**
  - Parallel odometry wheel (2.75") for consistent linear measurement  
  - Heading PID using inertial sensor  
  - Adjustable `maxDriveSpeed` / `maxTurnSpeed` per command  

- **Lady Brown Arm PID**
  - Rotation sensor feedback loop  
  - 3-position state machine (Rest, Loading, Scoring) triggered by controller inputs  

- **Autonomous Routines**
  - Red Rush Winpoint (fully enabled)  
  - Blue Rush, Red/Blue Non-Rush, Skills, and Solo Winpoints (all scaffolded for easy activation)  

- **Driver Controls**
  - Tank drive with hold/coast modes  
  - Intake forward/reverse + partial intake toggle  
  - Clamp, doinker, and intake-lift pneumatic toggles  
  - Lady Brown quick state-switch mapping  
  - Skills routine quick-start combo  

---

## Hardware Setup

**Drivetrain** (6 × 11W motors)  
- Left: `PORT2` (front, rev), `PORT1` (upper), `PORT10` (back, rev)  
- Right: `PORT3` (front), `PORT4` (upper, rev), `PORT9` (back)  

**Mechanisms**  
- Lady Brown Arm: `PORT13`  
- Upper Intake: `PORT19`  
- Lower Intake: `PORT11`  

**Sensors**  
- Inertial: `PORT5`  
- Lady Brown Rotation: `PORT8`  
- Parallel Odometry: `PORT6` (rev)  

**Pneumatics (3-wire)**  
- Clamp: `A`  
- Doinker: `B`  
- Intake Lift: `C`  

---

## File Structure

- `robot-config.h` — hardware definitions + setup  
- `main.cpp` — competition template, PID loops, autonomous & driver code  

---

## Controller Layout

| Control | Function |
|---|---|
| Axis3 / Axis2 | Tank drive |
| R1 | Intake (100%) |
| R2 | Outtake (75%) |
| B | Lower intake only |
| L1 | Clamp toggle |
| L2 | Doinker toggle |
| A | Intake lift toggle |
| Y / Right | Lady Brown state transitions |
| Up + X (hold) | Quick-start Skills |

Cooldown counters prevent rapid input repeat.  

---

## PID Details

- **Drivetrain Constants**:  
  `kP=0.78, kI=0.35, kD=0.575`  
  `turnkP=0.134, turnkI=0.011, turnkD=0.11`  

- **Lady Brown Constants**:  
  `LBkP=0.25, LBkI=0.0, LBkD=0.01`  
  - Rest ≈ 0°  
  - Loading ≈ 120/5°  
  - Scoring ≈ 655/5°  

- **Notes**: Integral used sparingly, zero-cross resets applied, per-move speed caps available.  

---

## Auton Slots

1. **Red Rush Winpoint** — active, tested, tuned.  
2. **Blue Rush Winpoint** — scaffolded (commented).  
3–4. **Red/Blue Non-Rush** — scaffolded, focused on stake scoring consistency.  
5. **Skills** — long routine scaffolded, requires tuning.  
6–7. **Solo Winpoints (Red/Blue)** — scaffolded, optimized for single robot WP.  

---

## Video Demo
Check out our autonomous + skills highlights:  
[![Auton Demo](https://img.youtube.com/vi/EQBw6I2zAq8/0.jpg)](https://www.youtube.com/watch?v=EQBw6I2zAq8)

---

## Development Notes

- Default auton is Slot 1 (Red Rush). Others commented for easy switching.  
- Delays (`task::sleep`) may need tuning for robot conditions.  
- PID tuned for 6-motor base + 2.75" odometry. Adjust if hardware changes.  

---

## Roadmap

- Replace delay-based logic with settle detection  
- Break out auton routines into modular functions  
- Expand odometry support for smoother arcs  

---

## Stack

- **Language**: C/C++  
- **Platform**: VEXcode Pro V5  
- **Hardware**: V5 Brain, 6-motor drivetrain, Lady Brown arm, intakes, pneumatics, inertial + rotation sensors  

---

## Safety + Setup

- Verify port mapping & motor reversals after any rebuild  
- Pneumatics reset to safe states on boot  
- Always re-tune PID after hardware/battery changes  

---

## Credit

Built by **Isaiah Thomas** for the 2024–2025 High Stakes season.  
Fork + adapt for learning purposes—please acknowledge original repo.

