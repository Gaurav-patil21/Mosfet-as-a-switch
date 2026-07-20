# Gate resistor

A gate resistor (Rg, or "gate stopper") is a series resistor placed between a switching device's driver (for example, a microcontroller, logic gate, or dedicated gate driver) and the MOSFET gate. It limits transient currents, damps parasitic oscillations, reduces EMI, and helps protect the driver and MOSFET during switching.

## Why gate resistors are essential
- Current limiting — A MOSFET gate behaves like a capacitor. During switching the driver must charge or discharge that capacitance, which can produce very high peak currents for short intervals. The gate resistor limits this peak current and protects the driver.
- Damping ringing & EMI — Fast switching plus stray PCB inductance forms an RLC resonant circuit that can produce ringing and radiated emissions. The resistor damps that resonance.
- Preventing shoot-through — In half-bridge and bridge topologies, controlling turn-on and turn-off speed prevents both devices (high-side and low-side) from conducting simultaneously, which reduces shoot-through current.
- Driver stability and protection — Limits stress on the driver and reduces the effect of gate-driver output inductance interacting with gate capacitance.

## How it works (quick formulas)
- Capacitive current: I = C · dV/dt. Example: if the effective gate capacitance is 1 nF and dv/dt = 1 V/ns (1e9 V/s), then I = 1 A peak.
- Gate-charge method: Ig ≈ Qg / tswitch, where Qg is the MOSFET gate charge (nC) from the datasheet and tswitch is the desired charging time. Then choose Rg ≈ Vdrive / Ig. Example: Vdrive=5 V, Qg=20 nC, tswitch=20 ns → Ig ≈ 1 A → Rg ≈ 5 Ω.
- RC time constant: τ = Rg · Cg_effective. Larger R slows switching (longer rise/fall times), reducing dv/dt and EMI but increasing switching losses.

## Selecting a gate resistor — practical guidance
- Typical ranges:
  - Microcontroller GPIO (weak driver): 10 Ω — 100 Ω. Common sweet spot: 47 Ω.
  - Strong gate driver (dedicated IC): 0.5 Ω — 10 Ω to allow fast switching with controlled peak current.
  - Noisy or sensitive layouts, or when EMI must be minimized: consider 100 Ω or more, but test for switching loss and thermal consequences.
- Trade-offs:
  - Larger Rg → less ringing & EMI, lower peak gate current, but slower switching → higher switching losses and potentially more heat in the MOSFET.
  - Smaller Rg → faster switching, lower switching losses, but higher dv/dt, more EMI, and greater ringing risk.
- Use the MOSFET datasheet (Qg, Ciss, Cgd) and the driver capability (peak source/sink current) to pick Rg quantitatively.

## Power dissipation and thermal note
- Energy per switching event (charge/discharge of gate cap): E = 0.5 · Cg · Vdrive^2 (J). Average power dissipated each switching cycle is Pavg = E · fsw.
- The resistor sees pulse currents; choose a resistor with an appropriate pulse rating. Average resistor power is usually small at low frequency, but check energy and duty cycle for higher switching frequencies.

Example: Cg_effective = 1 nF, Vdrive = 12 V, fsw = 100 kHz:
E = 0.5 · 1e-9 · 12^2 = 72 nJ per transition → Pavg ≈ 72 nJ · 100 kHz = 7.2 mW.

## PCB placement and layout
- Place the gate resistor as close as possible to the MOSFET gate pin (ideally right at the pad). This keeps the high di/dt loop small and isolates the gate from PCB trace inductance.
- Keep the driver-to-gate net short and avoid routing that increases loop area with the source or return.
- If using a Kelvin gate connection (separate power and sense), keep the resistor in the gate path right at the gate pad so the Kelvin sense remains valid.
- Consider small ferrite beads in series for extra EMI suppression or use snubbers if needed.

## Variations and advanced tips
- Two-resistor approach: A small resistor (1–10 Ω) close to the gate and a larger resistor near the driver can help damp local ringing while isolating driver output. Alternatively, place a small resistor at the gate and a larger one at the driver if sharing drivers.
- Miller effect (Cgd) matters in half-bridges — watch for unintended turn-on from dv/dt. Increasing Rg reduces dv/dt and the risk of Miller-induced spurious switching.
- Use a gate-to-source bleed resistor (100 kΩ–1 MΩ) to define gate off state and avoid floating gates at power-up.

## Worked examples
1) MCU driving a logic-level MOSFET:
- Given: Vdrive = 5 V, Qg = 15 nC, desired tswitch ≈ 50 ns → Ig ≈ 15 nC / 50 ns = 0.3 A → Rg ≈ 5 V / 0.3 A ≈ 17 Ω. Pick 22 Ω or 18 Ω; many designs use 10–47 Ω for MCU outputs.
2) Dedicated gate driver:
- Given: Vdrive = 12 V, Qg = 50 nC, tswitch ≈ 20 ns → Ig ≈ 2.5 A → Rg ≈ 12 V / 2.5 A ≈ 4.8 Ω → choose 4.7 Ω or 5 Ω.

## What to document/test on your board
- Measure gate waveform (rise/fall times and dv/dt) and switch current during transitions.
- Check for ringing amplitude at gate and drain; tune Rg and layout to reduce it.
- Verify switching losses and device temperature when increasing Rg (slower transitions increase losses).
- Confirm EMI emissions if your design must pass regulatory tests.

## References & further reading
- Check MOSFET datasheet (Qg, Ciss, Cgd) for exact gate-charge numbers.
- Vendor app notes (Infineon, Texas Instruments, ST, ON Semiconductor) cover gate driver selection, ESR/Rg trade-offs, and layout tips.
