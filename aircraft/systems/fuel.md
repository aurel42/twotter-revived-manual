---
title: Fuel system
review: draft
---

# Fuel system

## From the README

- Migrated to "modern fuel system" (MSFS 2024) architecture.
- Restored the IRL behavior of the wing tanks: the "Engine" position enables the electric pump that feeds fuel from the wing tanks into the main tanks.
- A valve will pause the transfer around 75-80% per target tank.
- The wing tanks "refuel" position does nothing.
- Wing tank pump failure warning lights illuminate when the pumps can't draw fuel.

## From the release notes

Verbatim from `docs/release-notes-*.md`, grouped by the section they appeared in. The tag is the version each item first appeared in.

### Fuel system

- Overhauled boost pumps and fuel selector for more accurate behavior. Restored missing audio, removed conflicting audio. *(v2024.2.142)*
- Worked around the sim driving engine-driven fuel pump pressure from shaft (propeller) RPM instead of Ng. The pump is now hard-off below 35 % Ng and ramps back in above 40 %, with a small pressure floor so combustion stays alive with a feathered or near-stopped prop. *(v2024.2.142)*
- Migrated to "modern fuel system" (MSFS 2024) architecture. *(v2024.2.x)*
- Restored the IRL behavior of the wing tanks: the "Engine" position enables the electric pump that feeds fuel from the wing tanks into the main tanks. *(v2024.2.x)*
- A valve will pause the transfer around 75-80% per target tank. *(v2024.2.x)*
- The wing tanks "refuel" position does nothing. *(v2024.2.x)*
- Wing tank pump failure warning lights illuminate when the pumps can't draw fuel. *(v2024.2.x)*
- Electric fuel pumps now take a moment to build pressure. *(v2024.3.183)*
- Feedback: Removed the mandatory boost pump requirement below 8000 ft density altitude. *(v2024.3.183)*

## L:vars

Variables worth binding to hardware, driving from FSUIPC / Axis&Ohs / SPAD.neXt,
or reading on an external display. **R** is read-only state; **R/W** can be set.
This is a selection, not the full list — the aircraft defines around a thousand
`DHC6_*` names, most of them internal.

| L:var | R/W | Meaning |
|---|---|---|
| `DHC6_FUEL_WING_TANK_L` | R/W | Left wing-tank selector |
| `DHC6_FUEL_WING_TANK_R` | R/W | Right wing-tank selector |
| `DHC6_WING_PUMP_ELEC_L` | R | Left wing electric pump running |
| `DHC6_WING_PUMP_ELEC_R` | R | Right wing electric pump running |
| `DHC6_WING_PUMP_WARN_L` | R | Left wing pump can't draw — the warning lamp |
| `DHC6_WING_PUMP_WARN_R` | R | Right wing pump warning |
| `DHC6_FUELBOOST_FWD_STATE` | R | Forward boost pump state |
| `DHC6_FUELBOOST_AFT_STATE` | R | Aft boost pump state |
