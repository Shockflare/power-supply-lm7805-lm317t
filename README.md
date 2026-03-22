# Dual Output Power Supply (5V & 3.3V)

A regulated bench power supply built on a breadboard using the LM7805 
and LM317T, capable of supplying both 5V and 3.3V simultaneously.

## Features
- Fixed 5V output via LM7805
- Adjustable 3.3V output via LM317T
- Simulated in LTSpice before breadboard build

## Components
| Component | Value | Purpose |
|-----------|-------|---------|
| LM7805    | 5 V    | Fixed 5 V regulator |
| LM317T    | 3.3 V     | Adjustable regulator |
| 1N5819 Schottky Diode | 40 V 1 A | Reverse polarity protection |
| Capacitor | 470 µF | Input bulk storage |
| Capacitor | 100 nF  | Input high frequency bypass |
| Capacitor | 100 nF | Output high frequency bypass |
| Capacitor | 100 µF  | Output bulk storage |
| Resistor  | 120 ohm | LM317T voltage divider |
| Resistor  | 120 ohm | LM317T voltage divider |
| Resistor  | 390 ohm | LM317T voltage divider |
| Resistor  | 100 ohm | LED current limiter |
| LED  | 3.4 V 20 mA    | 5 V Power indicator |
| 5 V Load  | 33 ohm | 5 V Load simulator |
| 3.3 V Load  | 22 ohm | 3.3 V Load simulator |

## How It Works
The LM7805 provides a fixed 5V output. The LM317T output voltage 
is set using the formula:

V_out = 1.25 × (1 + R2/R1)

For 3.3V: R1 = 240Ω, R2 = 390Ω

## Challenges & Debugging
- Found that the LM317T pinout (Input, ADJ, Output) was not in the 
  expected order — correcting this resolved incorrect output voltage.
- LM317T adjustment voltage divide incorrectly connect to board output
  and adjudment input of the LM317T in LTSpice simulation.
- Ignored drop out voltage on LM317T. LM317T required 3 V of input-to-output
  voltage drop.

## Results
- Measured 5V output: 5.02 V
- Measured 3.3V output: 3.29 V

## What I Learned

**LTspice simulation workflow:**
- Simulated the full circuit before building, which caught a wiring 
  error in the LM317T feedback network. The ADJ pin was incorrectly 
  connected — simulation made this visible before any hardware was 
  damaged.

**LED SPICE modeling:**
- Discovered that LTspice's generic diode model uses ~0.75V forward 
  voltage, which does not represent a real LED. Replaced the diode 
  model with a behavioral voltage source set to the LED's actual 
  forward voltage (3.2V) to get accurate current calculations.

**LM317T regulation mechanism:**
- Learned that the LM317T does not regulate to ground — it maintains 
  exactly 1.25V between its OUTPUT and ADJ pins. The output voltage 
  is set by a resistor divider, and I derived the formula from first 
  principles using Ohm's law and KVL rather than just applying it.

**Capacitor selection reasoning:**
- Learned to select capacitor values based on their specific job — 
  bulk storage electrolytics sized by C = I×Δt/ΔV, and 100nF ceramics 
  for high frequency bypassing on every IC power pin. Also learned 
  why electrolytics cannot be used in AC signal paths but are valid 
  on DC power rails where polarity never reverses.

**Debugging real hardware:**
- Built the circuit on a breadboard and measured only 1.4V output 
  from the LM317T. Diagnosed the cause by measuring each pin 
  individually with a multimeter and checking the resistor divider 
  connections. This matched the simulation debugging process.

**Power dissipation:**
- Calculated and observed heat dissipation in both the load resistors 
  and the L7805 regulator. Learned to apply P = V²/R and 
  P = (Vin-Vout) × I to predict thermal behavior before touching 
  components.

## Future Improvements
- Redesign for parallel regulators instead of regulators in series
- Design a PCB in KiCad with ground plane and proper component placement
- Replace linear regulators with a buck converter to reduce heat dissipation
  at high load currents (L7805 dissipates 1.76W at 250mA load)
- Add overcurrent protection using a current sense resistor and comparator
- Add voltage and current display using an INA219 current sensor and 
  OLED display
