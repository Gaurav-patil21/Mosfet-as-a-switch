## Overview
10-channel MOSFET switch PCB for controlling inductive, resistive, capacitive, and PWM loads using any MCU or logic voltage.

## Components
- IRLZ44N MOSFET
- 1N4007 Diode (flyback protection)
- 3mm LED with 2.2kΩ resistor (output indicator)
- 27Ω gate resistor
- 10kΩ pull-down resistor
- 6-pin JST connector
- Screw terminals (power/load)

- Reason behind taking this values is that i want to play a safe game rather than calculating and using fixed values of the resistors, i used standard values which works great 

## Source/Sink Configuration for MOSFET
### The Core Concepts

**Sourcing Current:** 
- The MOSFET is placed between the positive voltage supply and the load
- Current flows out of the MOSFET's drain and into the load
- Typically done using a P-Channel MOSFET or an N-Channel MOSFET operating on the high side

**Sinking Current:** 
- The MOSFET is placed between the load and ground
- Current flows out of the load, through the MOSFET's drain to source, and sinks into the ground
- Most common and straightforward configuration using an N-Channel MOSFET on the low side

### How MOSFETs Create Current Sinks/Sources

When a MOSFET operates in the saturation region (where V_DS > V_GS - V_th), it acts as a constant current sink or source:
- In this region, the current I_D flowing through the device is primarily controlled by the gate-to-source voltage (V_GS), not by the voltage across the load
- This allows the MOSFET to maintain a steady, constant current for applications like:
  - LED strings
  - Battery chargers
  - Active loads in IC amplifiers
