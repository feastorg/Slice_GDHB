# Gate-Driven H-Bridge Slice

Gate-driven Discrete H-Bridge Slice for BREAD.

## Introduction

[Short introduction on the project.]

## Usage

[Explain how to use the project. Include examples and screenshots if applicable.]

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

## Documentation

Please refer to the [docs](/docs) for more info.

---

<div align="center">
  <p>
    <img src="https://img.shields.io/badge/Developed_by-UWO--FAST--et--al-orange" alt="Developed by UWO-FAST et al.">
    <img src="https://img.shields.io/badge/Powered_by-Open_Source-blue" alt="Powered by Open Source">
  </p>
</div>
