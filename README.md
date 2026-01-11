# PLC-Based Automatic / Manual Tank Level Control with PID

## Project Description
This project is a PLC-based liquid tank level control application with **manual** and **automatic** operating modes.  
The system is designed with a focus on **safe operation**, **clear mode separation**, and **predictable control behavior**.

The project was developed using **Siemens TIA Portal** and demonstrates practical PLC programming, PID-based level control, and operator-oriented HMI design.

---

## Functional Overview

### Manual Mode
In manual mode, the operator directly controls the process:

- **Fill (Doldur)** starts tank filling
- **Drain (Boşalt)** starts tank draining
- Filling and draining cannot be active at the same time
- **Stop** immediately halts all active operations
- After Stop, a new operator command is required to restart

This prevents unintended restarts and conflicting actuator commands.

---

### Automatic Mode
Automatic mode enables closed-loop level control:

- **Fill (Doldur)** activates PID-based level control
- Tank level is regulated to the defined setpoint
- **Drain** command is disabled in this mode
- **Stop** disables the PID controller and safely stops the system

PID control is active only in automatic mode to avoid control conflicts.

---

## Mode Transition Logic
Mode transitions are strictly controlled to ensure safe operation:

- Mode changes are not allowed while the system is active
- If a mode change is requested during operation:
  - The system is forced into **Stop state**
  - A warning message is displayed on the HMI
- Mode selection is only permitted when the system is stopped

This approach prevents unsafe state transitions and unexpected process behavior.

---

## Control Architecture
The control logic follows a **state-based structure**:

- STOP  
- MANUAL  
- AUTOMATIC  

Key design principles:
- Manual and automatic logic paths are clearly separated
- Physical outputs are driven through internal memory states
- All outputs are deactivated in Stop condition

This structure improves readability, safety, and future scalability.

---

## HMI Design & Operator Interaction
The HMI acts as an **operator interface**, rather than a direct actuator controller.  
Operator commands are validated based on the current system state to ensure safe interaction.

![HMI Ekranı](https://github.com/MelihCimen482/plc-automatic-manual-tank-level-control-pid/blob/22520eaa7ec4f3e06735c3ee9265358332a3add8/Images/Ekran%20g%C3%B6r%C3%BCnt%C3%BCs%C3%BC%202026-01-11%20151057.png
)

---

### Control & Safety Interaction
- Operator actions are evaluated according to the active operating mode
- Invalid actions are blocked and communicated via HMI messages
- The Stop command is always available as a high-priority safety function

![HMI Ekranı](https://github.com/MelihCimen482/plc-automatic-manual-tank-level-control-pid/blob/22520eaa7ec4f3e06735c3ee9265358332a3add8/Images/Ekran%20g%C3%B6r%C3%BCnt%C3%BCs%C3%BC%202026-01-11%20151136.png)
---

### Mode-Dependent Control Logic
HMI control elements are enabled or restricted based on system state:

- Manual control buttons are disabled during automatic PID operation
- Automatic control commands are unavailable in manual mode
- Mode selection is only possible when the system is in Stop condition

This reduces operator error and prevents conflicting commands.

---

### Process Visualization
The HMI provides real-time visualization of key process variables:

- Tank level feedback
- Level setpoint
- Valve opening percentages
- System operating status

A separate analysis screen displays tank level trends, allowing observation of PID response and overall system behavior.

![HMI Ekranı](https://github.com/MelihCimen482/plc-automatic-manual-tank-level-control-pid/blob/22520eaa7ec4f3e06735c3ee9265358332a3add8/Images/Ekran%20g%C3%B6r%C3%BCnt%C3%BCs%C3%BC%202026-01-11%20150835.png)


## Technical Implementation Details

### Analog Signal Processing
- Tank level sensor outputs a **0–10V analog signal**
- The analog input module converts this signal to a **0–27648 integer value**
- This value is used as the process variable for PID control

![HMI Ekranı](https://github.com/MelihCimen482/plc-automatic-manual-tank-level-control-pid/blob/22520eaa7ec4f3e06735c3ee9265358332a3add8/Images/Ekran%20g%C3%B6r%C3%BCnt%C3%BCs%C3%BC%202026-01-11%20135507.png)
---

### PID Controller Execution
- PID control is executed inside a **cyclic interrupt organization block**
- Constant execution timing ensures stable and predictable PID behavior
  
![PID Control](https://github.com/MelihCimen482/plc-automatic-manual-tank-level-control-pid/blob/22520eaa7ec4f3e06735c3ee9265358332a3add8/Images/Ekran%20g%C3%B6r%C3%BCnt%C3%BCs%C3%BC%202026-01-11%20145526.png
)
---

### PID Safety Handling
- The PID controller is disabled when the system enters Stop state
- This prevents integral wind-up and ensures smooth restart behavior

---

## Tools & Technologies
- Siemens TIA Portal  
- Siemens PLC  
- SIMATIC HMI  
- PID Compact Controller  
- Cyclic Interrupt OB  

---

## Project Purpose
This project was developed as a **technical portfolio application** to demonstrate:

- Safe manual and automatic control logic
- Proper PID integration with deterministic execution
- Industrial-style mode handling
- Practical HMI design for operator interaction

---

## License
This project is intended for educational and portfolio demonstration purposes.
