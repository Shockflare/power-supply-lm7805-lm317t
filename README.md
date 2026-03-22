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

## Results
- Measured 5V output: X.XXV
- Measured 3.3V output: X.XXV

## What I Learned
- Difference between fixed and adjustable linear regulators
- Importance of bypass capacitors for stability
- How to read and apply IC datasheets

## Future Improvements
- Design a PCB using KiCad
- Add current limiting and short circuit protection
- Add a voltage/current display
