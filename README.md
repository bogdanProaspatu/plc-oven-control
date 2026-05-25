# 🏭 Distributed Automation and Control of an Industrial Furnace (Siemens LOGO! PLC)

This project is a complete engineering solution for automating industrial thermal processes. The system uses a **distributed control** architecture based on two **Siemens LOGO! 8** programmable logic controllers (PLCs), optimizing the heating process by separating execution logic from monitoring and safety logic.

---

## 🎯 1. Project Goal and Objectives
The project was designed to meet the rigorous requirements of industrial environments (metallurgy, ceramics, or heat treatment):
* **Thermal Precision:** Controlling temperature within a strictly defined range to ensure product quality.
* **Redundancy and Safety:** Implementing fault protocols that prevent equipment damage in case of hardware errors.
* **Modular Architecture:** Using two interconnected PLCs to reduce processing load and allow independent debugging.
* **Operator Interface (HMI):** A visual and audible signaling system for operating, error, and maintenance-required states.

---

## ⚙️ 2. System Architecture and Hardware
The system is split into two distinct processing units, galvanically interconnected through status signals ($Q \rightarrow I$).

### 2.1. PLC 1 - Thermal Process Control (FBD Logic)
This is the "power" unit that interacts directly with the analog sensors.
* **Inputs:** 1x analog temperature sensor (AI1 - PT100/TC).
* **Outputs:** 1x heating-element command (Q1).
* **Language:** **FBD (Function Block Diagram)** - optimal for analog signal processing.
* **File:** `Subproces_1.lsc`

### 2.2. PLC 2 - Monitoring and Safety (LAD Logic)
Handles the electrical side, interlocks, and operator protection.
* **Inputs:** Start/Stop, Reset, overheating sensor, PLC1 status signal.
* **Outputs:** Audible alarm, Maintenance indicator, Progressive cooling system.
* **Language:** **LAD (Ladder Diagram)** - the industrial standard for relay logic.
* **File:** `Subproces2LADFinal.lld`

---

## 🧠 3. Detailed Operating Logic

### 3.1. Thermal Control Algorithm
A **hysteresis** logic (ON/OFF with thresholds) was implemented to avoid premature wear of the contactors:
* **Minimum Threshold:** Activating the heating elements when the temperature drops below the set limit.
* **Maximum Threshold:** Disconnecting the heating when the process temperature is reached.

### 3.2. Progressive Stepped Cooling
To protect the integrity of the processed materials, the system does not stop cooling abruptly but uses a three-step protocol managed by timers:
1. **Step 1:** Maximum ventilation immediately after the cycle finishes.
2. **Step 2:** Reducing intensity to stabilize the temperature.
3. **Step 3:** Slow cooling down to the safe threshold for unloading.

---

## ⚠️ 4. Safety and Alarm Protocols
Safety is the central pillar of this project, with 4 error filters implemented:

1. **Heating Watchdog:** If the heating output is active but AI1 does not report a temperature increase within 30 seconds, the system declares a "Faulty Heating Element".
2. **Noise Filter (Debouncing):** The overheating alarm is triggered only if the sensor reports the error continuously for more than 5 seconds.
3. **Cycle Counter (Maintenance):** Every 5 cycles, PLC2 blocks startup and turns on a service indicator.
4. **Safety Interlock:** Any critical error requires a **Manual Reset (I16)**; automatic restart is prohibited to force operator inspection.

---

## 📊 5. Implementation in LOGO! Soft Comfort
The project includes the complete diagrams developed in version 8.3:
* **FBD:** Use of analog comparison blocks and signal amplifiers.
* **LAD:** Implementation of self-holding circuits and *Off-Delay* and *On-Delay* timers.
* **P&ID:** Symbolic representation of the process flow (Piping and Instrumentation Diagram).

---

## 🛠️ 6. Installation and Running Guide
1. **Software:** Install **LOGO! Soft Comfort V8.3** or newer.
2. **Configuration:** Load `Subproces_1.lsc` onto the first PLC and `Subproces2LADFinal.lld` onto the second.
3. **Hardware:** Make the physical connections between the Q output of the first PLC and the I input of the second (per the communication diagram in the PDF).
4. **Simulation:** Use the simulation mode (F3) to vary the AI1 analog input and observe the sequences being triggered.

---

## 👨‍💻 Author
**Nicolae-Bogdan Proaspătu**
*Automation and Applied Informatics student, Technical University of Civil Engineering Bucharest*
Academic year 2025–2026

---

## ⚖️ License
Developed exclusively for educational purposes as part of the **Programmable Logic Controllers (PLC) Applications** course.
