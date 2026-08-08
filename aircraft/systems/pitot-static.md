---
title: Pitot-static
review: draft
---

# Pitot-static

The altimeter and VSI read static pressure; the airspeed indicator reads the
difference between pitot and static. Pitot *heat* is covered under
[Ice protection](ice-protection.md).

## From the README

*Redistributed from the former Cockpit instruments page.*

- Implemented Captain-side altimeter with aneroid simulation, a servo motor that drives needle and drum with realistic latching mechanism, the baro knob is mechanically linked. Based on IDC Encoding Altimeter 28V.
- Added calibration differences between mirrored instruments (altimeters, HSI, RMI).

## From the release notes

Verbatim from `docs/release-notes-*.md`, grouped by the section they appeared in. The tag is the version each item first appeared in.

### Cockpit instruments

- [NEW] Implemented Captain-side altimeter with aneroid simulation, a servo motor that drives needle and drum with realistic latching mechanism, the baro knob is mechanically linked. Based on IDC Encoding Altimeter 28V. *(v2024.2.x)*

### Plans

- Pitot-static system *(v2024.2.x)*

### Instruments & controls

- **Calibration errors.** Work has started on inducing variations between mirrored instruments (altimeters, HSI, RMI). *(v2024.3.47)*

## L:vars

Variables worth binding to hardware, driving from FSUIPC / Axis&Ohs / SPAD.neXt,
or reading on an external display. **R** is read-only state; **R/W** can be set.
This is a selection, not the full list — the aircraft defines around a thousand
`DHC6_*` names, most of them internal.

| L:var | R/W | Meaning |
|---|---|---|
| `DHC6_ALT_CPT_DISPLAYED` | R | Captain's altimeter as displayed — servo lag and latching included |
| `DHC6_ALT_CPT_ANEROID` | R | The aneroid's own reading, before the servo |
| `DHC6_ALT_CPT_DRUM_1000` | R | 1000 ft drum position |
| `DHC6_ALT_CPT_DRUM_10000` | R | 10000 ft drum position |
| `DHC6_ALT_FO_BIAS_FT` | R | FO altimeter calibration offset from the captain's |
