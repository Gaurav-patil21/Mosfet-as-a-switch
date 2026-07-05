# Flyback (Freewheeling) Diodes — protecting a MOSFET from inductive back‑EMF

When a MOSFET switches an inductive load (relay, solenoid, motor, coil, etc.), the inductor resists changes in current and can produce a high-voltage spike (back‑EMF) when the current is interrupted. That spike can damage the MOSFET or other semiconductors. A flyback (freewheeling) diode safely clamps that voltage and provides a current path for the inductor's energy.

## How back‑EMF occurs (brief)
An inductor stores energy in its magnetic field: E = 1/2 · L · I^2. When the switch (MOSFET) opens quickly, the inductor attempts to keep the current flowing. The only way to maintain current is for the voltage across the inductor to change — potentially producing a large voltage spike. Without a clamp, that spike can exceed device voltage ratings and cause avalanche, arcing, or destruction.

## How a flyback diode works
- A diode is placed across (in parallel with) the inductive load.
- When the MOSFET is ON, current flows normally through the load.
- When the MOSFET turns OFF, the diode becomes forward biased and provides a low‑voltage path for the inductive current to circulate until the energy dissipates.
- This clamps the voltage to roughly Vsup + diode_forward_voltage (for a low‑side switch) instead of allowing a large spike.

## Typical wiring (low‑side switching)
- Low‑side MOSFET (MOSFET between load and ground): place the diode across the load with the diode cathode to V+ and anode to the MOSFET drain/load node.
- High‑side switching: orientation is reversed accordingly.
- Keep wiring short and loop area small to reduce parasitic inductance.

ASCII (low‑side):
V+ ---+----+ Load +---+--- Node A -----+ MOSFET --- GND
       |    |        |                |
       +----|<-------+  Flyback diode  (cathode to V+,
            diode                      anode to Node A)

## Selecting a diode
- Continuous/peak current rating: diode must handle the inductor current (at least the steady current; consider peak and initial surge).
- Reverse voltage (VRRM): diode reverse rating must exceed the supply voltage plus any transients.
- Speed:
  - Standard silicon diodes (1N400x) are OK for low switching speeds (< a few kHz) and small inductive loads.
  - Schottky diodes have lower forward drop and faster recovery — preferred for faster switching or higher efficiency.
  - Fast / ultra‑fast recovery diodes are useful for PWM/high‑frequency switching.
- Power dissipation: compute average diode power from dissipated in each switch event (see calculation below) and ensure proper thermal handling.

Example practical parts:
- Small relays/solenoids: 1N4001–1N4007 family (if switching is slow).
- Faster or high‑current: SR560 or similar Schottky diodes, or an appropriate rated fast diode.

## Basic energy / power calculation
- Energy stored: E = 1/2 · L · I^2 (joules)
- If the switch opens once and energy is dissipated through the diode, the diode must absorb that single pulse energy.
- For repetitive switching at frequency f:
  - Average power dissipated in the diode ≈ E · f = (1/2 · L · I^2) · f
- Ensure diode average power rating and MOSFET rating accommodate this. For short pulses, consider pulse current rating as well.

## Alternatives and complements
- RC snubber (resistor + capacitor) across the load — reduces spike and shapes dissipation (useful if you want a controlled RC time constant).
- TVS (transient voltage suppressor) diodes — clamp to a specific voltage, useful for higher-speed transients.
- Active clamp or energy recovery circuits — used in advanced designs to recover inductor energy.
- Combination: a diode plus an RC snubber or TVS can give improved suppression for noisy or high‑frequency systems.

## Practical tips
- Place the diode physically close to the inductor/load to minimize stray inductance in the clamp loop.
- Use short, thick traces/wires for the diode path.
- Check MOSFET avalanche ratings but prefer an explicit clamp (diode or TVS) rather than relying on avalanche.
- Verify thermal dissipation and, if required, use a diode with a heat sinking arrangement for high power.
- When switching motors, consider additional EMI suppression and proper grounding.

## Example: MOSFET low‑side switch with a flyback diode
- N‑channel MOSFET between node A (load negative) and ground.
- Diode cathode to the positive supply, diode anode to node A.
- When MOSFET turns off, current flows from node A → diode → supply → load until the current decays.

## References and further reading
- Basics of inductors and flyback diodes (textbook sections or electronics handbooks)
- Component datasheets: check forward current, reverse voltage, and thermal limits for chosen diode.
