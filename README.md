# PLC-Based Automatic / Manual Tank Level Control with PID

## Project Overview
This project is a PLC-based liquid tank level control system designed with **manual and automatic operating modes**.  
The system focuses on **safe operation**, **clear mode separation**, and **industrial-style control logic**.

The project was developed using **Siemens TIA Portal** and includes manual control, PID-based automatic level regulation, stop logic, and HMI interaction.

---

## System Operating Modes

### Manual Mode
- **Fill (Doldur)** button starts manual filling.
- **Drain (Boşalt)** button starts manual draining.
- Filling and draining cannot be active at the same time.
- **Stop** immediately stops the process.
- After Stop, a new operator command is required to restart the system.

This behavior prevents unintended restarts and ensures operator safety.

---

### Automatic Mode
- Pressing the **Fill (Doldur)** button enables **PID level control**.
- The PID controller regulates the tank level automatically.
- **Drain button has no effect** in automatic mode.
- Pressing **Stop** disables the PID controller and stops the system safely.

PID control is active **only in automatic mode**.

---

## Mode Change & Safety Concept
- Mode changes are **not allowed while the system is active** (manual operation or PID control).
- If a mode change is requested while the system is running:
  - The system is forced into **Stop state**
  - A warning message is displayed on the HMI
- Mode changes are only allowed when the system is in **Stop condition**.

This approach prevents unsafe transitions and unexpected actuator behavior.

---

## HMI Design
The HMI is designed with a clear separation between **system operation** and **process analysis**.  
All HMI texts are displayed in **Turkish**, considering local operator usage, while the control logic remains language-independent.

### Main Control Screen
The main screen is used for **direct operator control** and real-time monitoring.

- Manual / Automatic mode selection via selector switch
- **Fill (Doldur)** and **Drain (Boşalt)** buttons for manual operation
- High-priority **Stop** button that safely stops the system and resets active states
- Real-time display of valve opening percentages, tank level setpoint, and current level
- Context-based status and warning messages for safe operation

The screen prevents conflicting commands and provides clear feedback to the operator.

### Level Analysis Screen
The analysis screen is dedicated to **monitoring system behavior** without affecting control.

- Real-time tank level trend visualization
- Observation of PID response and level stability over time
- Simple layout for fast interpretation

### Design Approach
- Separation of control and analysis functions
- Operator-oriented and safety-focused design
- Clear visualization of critical process variables

---

## Control Logic Architecture
- State-based control structure:
  - STOP
  - MANUAL
  - AUTOMATIC
- Manual and automatic control paths are separated to avoid conflicts.
- All physical outputs are conditioned through internal memory bits for safe shutdown behavior.

---

## Technical Design Decisions

### Analog Signal Processing
- The tank level sensor provides a **0–10V analog signal**.
- The analog input module converts this signal to a **0–27648 integer value**.
- This value is used as the process variable for PID control.

### PID Execution
- The PID controller is executed inside a **cyclic interrupt OB**.
- This ensures a constant execution time, which is critical for stable PID behavior.

### PID Safety Handling
- On **Stop condition**, the PID controller is disabled.
- This prevents integral wind-up and ensures smooth restart behavior.

---

## Used Technologies
- Siemens TIA Portal
- Siemens PLC
- PID Compact Controller
- SIMATIC HMI
- Cyclic Interrupt OB

---

## Project Purpose
This project was created as a **portfolio and reference application** to demonstrate:
- Safe manual and automatic control logic
- Proper PID integration
- Industrial-style mode handling
- Operator-oriented HMI design

---

## License
This project is intended for educational and portfolio purposes.
