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

## Documentation

Please refer to the [docs](/docs) for more info.

---

<div align="center">
  <p>
    <img src="https://img.shields.io/badge/Developed_by-UWO--FAST--et--al-orange" alt="Developed by UWO-FAST et al.">
    <img src="https://img.shields.io/badge/Powered_by-Open_Source-blue" alt="Powered by Open Source">
  </p>
</div>
