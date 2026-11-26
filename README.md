# Gate-Driven H-Bridge Slice

Gate-driven Discrete H-Bridge Slice for BREAD.

## Introduction

[Short introduction on the project.]

## Usage

[Explain how to use the project. Include examples and screenshots if applicable.]

## Ratings

| Item | Rating |
| --- | --- |
| Motor current | 10 A continuous |
| VBB (motor supply, J2.3 to J2.1) | 24 V max |
| Logic | 5 V from the slice bus (Nano and A4957 VDD) |

- **Fuse the motor supply.** An inline 15 A fuse in the VBB lead, outside
  the board, is required. The board has no fuse, no current limit or
  current sense, and no reverse-polarity protection; D2 (SMCJ26A) clamps
  VBB transients only (about 35 V at 10 A, under the FETs' 40 V).
- **Use synchronous rectification.** At 10 A a body diode carrying the
  freewheel current dissipates about 4 W. Drive leg A at 20 kHz with
  complementary AHI/ALO (Timer1 OC1A/OC1B) and hold leg B static for
  direction.
- **Ramp the duty cycle (soft start).** The bridge has 440 uF of bulk
  capacitance and no inrush or current limiting.
- **Order 1 oz copper on all four layers** (1.6 mm); the copper is sized
  for 10 A at a 20 C rise on 1 oz.
- PGND (FET sources, bulk caps, J2.1) joins the logic GND only at NT2,
  beside U2.

No firmware ships with this board yet.

## Nano pin map

| A4957 input | Net | Nano pin |
| --- | --- | --- |
| AHI (leg A high side) | AHI1 | D9 (Timer1 OC1A) |
| ALO (leg A low side) | ALO1 | D10 (Timer1 OC1B) |
| BHI (leg B high side) | BHI1 | D11 |
| BLO (leg B low side) | BLO1 | D6 |
| FAULT (open drain, low = fault) | FAULT1 | A0 |
| RESET (low = sleep) | RESET1 | A1 |

AHI and BHI swapped pins in November 2026 (AHI was on D11, BHI on D9), so
Timer1's two outputs drive both switches of leg A.

## Bill of materials

`documentation/BOM_GDHB_v0.ods` and `documentation/BREAD_Slice.csv` predate
this revision (they list the old parts and counts) and are stale. Take the
BOM from the schematic (KiCad: Tools > Generate BOM, or
`kicad-cli sch export bom`); the MPN field carries the part numbers.

## Design notes

- **C9 (VREG) is 10 uF, not 20 x CBOOT after DC bias.** The A4957
  datasheet (A4957-DS Rev. 2, "VREG Capacitor Selection", p. 10) sizes
  the VREG capacitor to limit the VREG drop when a low-side turn-on and
  a bootstrap recharge come together, and calls 20 x CBOOT "a
  reasonable value". With CBOOT = 330 nF (C7, C8) that is 6.6 uF.
  C9 (Murata GRM31CR71E106KA12L, 10 uF 25 V X7R 1206) keeps about 3.5 uF at 13 V on KEMET's
  DC-bias curve for the comparable C1206C106K3RACTU (-65%), about
  2.9 uF at VREG max (13.9 V) and -10% tolerance: 9 to 11 x CBOOT
  (313 nF at 13 V).
  The drop it has to hold is one low-side gate charge plus one
  bootstrap recharge, 2 x 98 nC (CSD18512Q5B Qg max at 10 V): 56 mV
  at 3.5 uF, 68 mV at 2.9 uF. VREG is 12.65 V minimum and its
  undervoltage lockout trips at 5.85 V maximum (VREGUVOFF, p. 5), so
  the margin is over 6.7 V. Charging both bootstrap capacitors from
  0 V at once out of C9 alone (2 x 330 nF x 12 V) would pull VREG
  down 2.7 V, still about 4 V above the lockout. The regulator's
  average load at 20 kHz with one leg switching (2 x 98 nC x 20 kHz,
  3.9 mA) is inside the 8 mA its 12.65 V minimum is specified at
  (p. 4).
- **NT2 (the only GND-PGND join) sits beside U2, not at the bulk
  capacitor's negative.** The A4957 layout notes (p. 11, first bullet,
  and Figure 2) return GND and the MOSFET sources separately to the
  negative of the motor-supply filter capacitor. Here that would tie
  GND at C5.2 or C6.2, 25-35 mm away through the VBB/PGND planes, and
  carry the low-side gate returns through it: the GLA/GLB gate loops
  would grow from about 24/28 mm2 to 177/529 mm2 (C5.2) or 997/537 mm2
  (C6.2). Beside U2, NT2 lands on the PGND pour that holds cell A's
  ceramic negatives (C16.2 is 6.2 mm away), which keeps those loops
  small. The datasheet calls its layout notes "only recommendations"
  and asks for each design to be tested at maximum current (p. 11):
  check the GND-PGND difference and low-side VGS ringing at 10 A.

## Documentation

Please refer to the [docs](/docs) for more info.

---

<div align="center">
  <p>
    <img src="https://img.shields.io/badge/Developed_by-UWO--FAST--et--al-orange" alt="Developed by UWO-FAST et al.">
    <img src="https://img.shields.io/badge/Powered_by-Open_Source-blue" alt="Powered by Open Source">
  </p>
</div>
