# Gate resistor

A gate resistor is a series resistor placed between a switching device's driver (like a microcontroller) and its gate. It limits peak current, prevents parasitic oscillations, dampens EMI, and protects the driver. Its value balances switching speed against thermal loss.

## Why Gate Resistors are Essential
**Current Limiting** - A gate acts like a capacitor; when it switches, it momentarily pulls a massive inrush current. The resistor restricts this current, protecting the driver.

**Damping Ringing & EMI** -  Fast switching combined with stray PCB inductance forms a resonant circuit that causes destructive "ringing" and electromagnetic interference (EMI).

**Preventing Shoot-Through** -  In half-bridge topologies, controlling the speed of turn-on and turn-off prevents both high-side and low-side devices from turning on simultaneously.

## Which gate resistor we are using ?
For a microcontroller to logic-level MOSFET connection, the best series gate resistor is 10 Ω to 100 Ω. A 47 Ω resistor is the ideal "sweet spot." It safely limits peak inrush current without making the switching transition too slow.
