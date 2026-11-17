# ⚡ PWM-Based DC Motor Speed Controller Using a 555 Timer IC

[![Course](https://img.shields.io/badge/Course-EEE%20234.1-blue.svg)]()
[![Department](https://img.shields.io/badge/Department-EEE%2C%20Southeast%20University-green.svg)]()
[![IC Used](https://img.shields.io/badge/IC-NE555%20%7C%20IRFZ44N-orange.svg)]()

## 📌 Project Overview
Controlling the speed of a DC motor efficiently is crucial in robotics and industrial automation. Traditional methods using a variable resistor (rheostat) waste significant power as heat and reduce motor torque at lower speeds. 

To overcome these limitations, this project implements **Pulse Width Modulation (PWM)** using an **NE555 Timer IC** configured in astable mode. By rapidly switching the DC power supply ON and OFF and varying the **Duty Cycle (1% to 99%)**, the circuit smoothly regulates the average voltage delivered to the motor—maintaining high torque and energy efficiency without overheating.

* **Course:** EEE 234.1 — Energy Conversion Laboratory
* **Institution:** Department of Electrical & Electronic Engineering (EEE), Southeast University
* **Author:** MD Ibrahim Khan (Group 5)

---

## 🛠️ Components Required

* **Timer IC — NE555 (U1)**
  * **Quantity:** 1
  * **Function:** Generates the adjustable PWM switching signal.
* **Power MOSFET — IRFZ44N (Q1)**
  * **Quantity:** 1
  * **Function:** Acts as a high-power electronic switch to drive the DC motor.
* **DC Motor — Standard DC Motor**
  * **Quantity:** 1
  * **Function:** Target load whose speed is being regulated.
* **Potentiometer — RV1 (100 kΩ)**
  * **Quantity:** 1
  * **Function:** Varies the pulse width / Duty Cycle.
* **Switching Diodes — 1N4148 (D1, D2)**
  * **Quantity:** 2
  * **Function:** Separates the charging and discharging paths of timing capacitor $C_1$.
* **Flyback Diode — 1N4148 (D3)**
  * **Quantity:** 1
  * **Function:** Protects the MOSFET from inductive Back-EMF voltage spikes.
* **Timing Capacitor — C1 (10 nF)**
  * **Quantity:** 1
  * **Function:** Sets the PWM oscillation frequency.
* **Filter Capacitor — C2 (10 nF)**
  * **Quantity:** 1
  * **Function:** Stabilizes the control voltage pin (Pin 5).
* **Gate Resistors — R1 (1 kΩ), R2 (1 kΩ)**
  * **Quantity:** 2 (1 each)
  * **Function:** $R_1$ limits gate current spikes from Pin 3; $R_2$ acts as a pull-down resistor.
* **Power Supply — B1 (6V / 12V DC)**
  * **Quantity:** 1
  * **Function:** Main DC power source for the circuit and motor.

---

## ⚙️ Working Principle

1. **PWM Signal Generation:**  
   The NE555 Timer IC operates in **Astable Mode**. Steering diodes **D1** and **D2** isolate the charging and discharging paths of the timing capacitor **C1** through potentiometer **RV1**. Adjusting RV1 alters the ratio between the charging time ($T_{ON}$) and discharging time ($T_{OFF}$), allowing smooth duty cycle variation from **~1% to ~99%** at a nearly constant frequency.

2. **MOSFET High-Current Switching:**  
   The PWM output from **Pin 3** drives the Gate of the **IRFZ44N N-Channel MOSFET** through current-limiting resistor **R1**. When the PWM pulse is HIGH, the MOSFET switches ON and powers the motor; when LOW, the MOSFET turns OFF completely via pull-down resistor **R2**.

3. **Smooth Speed Regulation:**  
   Because the switching occurs at a high frequency, the motor responds to the **average DC voltage** rather than individual pulses:
   $$\text{Average Voltage } (V_{\text{avg}}) = V_{\text{supply}} \times \text{Duty Cycle}$$
   This enables precise speed control from near-zero RPM to full speed without sacrificing torque.

---

## 🔌 Circuit Connections Summary

* **NE555 Timer (U1):**
  * **Pin 1:** Connected to Ground (`GND`).
  * **Pin 4 & Pin 8:** Connected to `+VCC` (+6V / +12V).
  * **Pin 5:** Decoupled to `GND` via capacitor **C2 (10 nF)**.
  * **Pin 2 & Pin 6:** Shorted together and connected to the junction of **D2 (Anode)** and **C1 (10 nF)** to `GND`.
* **Duty Cycle Control Network:**
  * **Pin 7:** Connected to **D1 (Anode)** and **D2 (Cathode)**.
  * **Potentiometer (RV1):** Middle wiper pin connects to **D1 (Cathode)** and **D2 (Anode)**; outer terminals connect across Pin 7 and `+VCC`.
* **MOSFET & Motor Stage:**
  * **Pin 3 (Output):** Connects to MOSFET Gate (`G`) via **R1 (1 kΩ)**.
  * **MOSFET (Q1):** Gate pulled down to `GND` via **R2 (1 kΩ)**; Source (`S`) to `GND`; Drain (`D`) to Motor Negative (`-`) terminal.
  * **Motor & Flyback Diode (D3):** Motor Positive (`+`) connects to `+VCC`. Diode **D3** is placed in reverse-parallel across the motor terminals (Anode to Drain, Cathode to `+VCC`).

---

## 💡 Key Design & Engineering Highlights

* **Why NE555 over a Microcontroller?** Provides a low-cost, purely analog, reliable hardware PWM solution without needing firmware programming.
* **Why Steering Diodes (D1 & D2)?** A standard 555 astable circuit cannot achieve a duty cycle below 50%. Adding D1 and D2 unlocks the full **1%–99%** speed control range.
* **Why IRFZ44N MOSFET?** Pin 3 of the 555 IC can only source/sink ~200 mA, which is insufficient for a DC motor. The MOSFET safely handles high motor current demands.
* **Why Flyback Diode (D3)?** Absorbs high-voltage inductive kickback (Back-EMF) generated when the motor coils switch off, preventing MOSFET breakdown.

---

## 📂 Repository Files
* `Conversion project report (1)_2.pdf` — Complete project report including circuit schematic, hardware prototype photos, and technical Q&A[cite: 3, 13, 14, 15, 16, 17].
