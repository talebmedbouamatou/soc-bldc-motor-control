# ⚡ SoC-Based Control of a BLDC Motor

![BLDC Motor Control](https://img.shields.io/badge/Project-BLDC%20Motor%20Control-1f6feb?style=flat-square)
![MATLAB Simulink](https://img.shields.io/badge/Modeling-MATLAB%2FSimulink-orange?style=flat-square)
![KiCad](https://img.shields.io/badge/PCB-KiCad-314CB0?style=flat-square)
![Xilinx Vivado](https://img.shields.io/badge/FPGA-Xilinx%20Vivado-5C2D91?style=flat-square)
![Platform](https://img.shields.io/badge/Platform-ZYBO%20Z7--10-2ea44f?style=flat-square)

##  Project overview

This project explores the development of a **sensorless speed-control system** for a brushless DC motor, also known as a **BLDC motor**. It brings together three parts of an embedded motor-control system: motor modeling and simulation, power-board design, and FPGA-based control.

The controller uses **six-step commutation** to drive the motor. At each step, two motor phases are powered while the third phase is left unpowered. The voltage on this unpowered phase is monitored to estimate the rotor position without using physical position sensors.

The system is designed around a **Xilinx Zynq-7000 SoC** on a **ZYBO Z7-10** board. The Zynq combines an ARM processor with programmable FPGA logic, allowing software and hardware functions to work together.

## ⚙️ How the control system works

At startup, the motor is driven using a forced open-loop sequence. This is needed because the Back-EMF signal is too weak at very low speed to estimate the rotor position reliably.

Once the motor reaches a suitable speed, the controller detects **zero crossings** in the Back-EMF of the unpowered phase. These events help estimate rotor position and determine when to switch to the next commutation step. PI regulators are used to control motor current and speed.

The work is divided between the two parts of the Zynq SoC:

- **ARM processor:** runs the control software, including the PI regulators, rotor-position estimation, and commutation logic.
- **FPGA logic:** generates PWM signals, applies switching dead time, and manages the six-step switching outputs.

A dedicated power board drives the motor and conditions the phase-voltage signals used by the sensorless control method.

##  Repository contents

The project files are organized into three archives:

###  `BLDC_Motor_Modeling_Taleb&Badr.zip`

Contains the MATLAB/Simulink modeling and simulation work. This part represents the electrical and mechanical behavior of the motor and is used to study its response and the proposed control strategy.

###  `Carte_a_Mosfet_Taleb&Badr.zip`

Contains the design files for the MOSFET power board. The board provides the three-phase power stage required to drive the motor, along with circuitry for conditioning the phase-voltage signals used by the controller.

### ️ `sensorless_bldc.zip`

Contains the **Vivado project for the FPGA-based control system**. It covers the programmable-logic part of the Zynq design and its integration with the ARM processor for BLDC motor control.

## ️ Main tools and technologies

- MATLAB/Simulink — motor modeling and simulation
- KiCad — electronic schematic and PCB design
- Xilinx Vivado — SoC and FPGA development
- ZYBO Z7-10 and Zynq-7000 — hardware platform
- VHDL and C — FPGA logic and processor software
- PWM, PI control, six-step commutation, and Back-EMF sensing

## ✅ Project progress

The project includes motor modeling and simulation, power-board design, and a Vivado FPGA project for the control architecture.

**The complete sensorless control system has not yet been fully validated experimentally on the real motor.** Testing the integrated system in closed-loop operation remains future work.
