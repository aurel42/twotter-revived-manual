---
title: Bleed air and ice protection
review: draft
---

# Bleed air and ice protection

## From the README

The Twotter is certified for FIKI (flight into known icing conditions). It has a beefy de-icing system consisting of leading edge boots on the wings and stabilizers, heated propeller edges and engine ice protection. Because of limitations of the sim, all these systems collapse into one single value that controls the structural icing of the aircraft and its effects on the flight model. I still tried to make each component matter.

- Each boot cycle takes about 16s, and in the last 8s, the blue overhead stab lights will come on.
- A single boot cycle can be triggered using the MANUAL switch position.
- In AUTO mode, boots will be actuated every 3m (SLOW) or every 1m (FAST).
- At the end of each boot cycle, a certain percentage of the ice is shed.
- Activating Prop Heat increases the percentage (yeah, that's the unrealistic part).
- Activating Intake Anti-Ice increases the percentage (same).
- Extending Intake Deflectors protects the engine from ingesting ice that is shed by Intake Anti-Ice (as part of a future engine damage modelling)

Notes: Bleed Air switches don't need to be "on" to extend the deflector. The extension mechanism gets its air upstream. At least one switch needs to be "on" to use the de-icing boots or cabin heating (not implemented yet).

Limitations: MANUAL mode does not behave authentically, it just starts a single boot cycle. IRL, in manual mode, the two extra switches (stab left/right, wing inner/outer) SOMEHOW control the boots directly, but I couldn't find out how, also, it would be complete theater, because of the "one single value" restriction.

Known issue: During the boot cycle, the LOW PNEUMATIC PRESS light can come on temporarily. The boot cycle will complete anyway.

- Rudimentary bleed-air "simulation": per-engine source PSI, manifold PSI after the cockpit shutoff switches, and demand from downstream consumers (deflector, deice boots).
- Pneumatic de-ice boot system with AUTO and MAN cycle modes. AUTO runs continuous cycles; MAN fires a single cycle and the switch snaps back.
- Windshield heat circuit mapped to separate DC buses (left and right pane independently).
- Icing accretion statistics on the EFB Flight tab.
- Inertial separator deflector deployment now needs bleed-air pressure.
- Windshield heat is now thermostat-controlled: instead of running flat-out whenever the switch is on, it warms the glass and then cycles to hold temperature. The underlying thermal model is minimal — in flight in cool air the heat stays effectively always-on, so it never interferes with the sim's internal windshield-icing effect (which the mod can't actually see). The thermostat state and modelled window temperature are shown on the EFB Debug tab.
- CCM applied his signature 400% icing bump, it remains to be tested whether the de-icing can keep up.
- [CHANGED] The bleed system is now a proper pneumatic model: per-engine source PSI, motorized cockpit shutoff valves, an unregulated duct, and a regulated de-ice manifold, with the POH 25/20 PSI protections. Cabin heat is fed from bleed through the duct, and a DUCT OVERHEAT caution trips above 125 °C (pull RAM AIR to clear).

## From the release notes

Verbatim from `docs/release-notes-*.md`, grouped by the section they appeared in. The tag is the version each item first appeared in.

### What's new since v2024.1.8

- Added pneumatic de-ice boots with AUTO and MAN cycle modes. *(v2024.2.x)*

### Bleed air & ice protection

- Each boot cycle takes about 16s, and in the last 8s, the blue overhead stab lights will come on. *(v2024.2.x)*
- A single boot cycle can be triggered using the MANUAL switch position. *(v2024.2.x)*
- In AUTO mode, boots will be actuated every 3m (SLOW) or every 1m (FAST). *(v2024.2.x)*
- At the end of each boot cycle, a certain percentage of the ice is shed. *(v2024.2.x)*
- Activating Prop Heat increases the percentage (yeah, that's the unrealistic part). *(v2024.2.x)*
- Activating Intake Anti-Ice increases the percentage (same). *(v2024.2.x)*
- Extending Intake Deflectors protects the engine from ingesting ice that is shed by Intake Anti-Ice (as part of a future engine damage modelling) *(v2024.2.x)*
- [NEW] Rudimentary bleed-air "simulation": per-engine source PSI, manifold PSI after the cockpit shutoff switches, and demand from downstream consumers (deflector, deice boots). *(v2024.2.x)*
- [NEW] Pneumatic de-ice boot system with AUTO and MAN cycle modes. AUTO runs continuous cycles; MAN fires a single cycle and the switch snaps back. *(v2024.2.x)*
- [NEW] Windshield heat circuit mapped to separate DC buses (left and right pane independently). *(v2024.2.x)*
- [NEW] Icing accretion statistics on the EFB Flight tab. *(v2024.2.x)*
- [NEW] Inertial separator deflector deployment now needs bleed-air pressure. *(v2024.2.x)*

### Bleed air & pneumatics

- Reworked the bleed system into a pneumatic model with motorized shutoff valves, an unregulated duct, and a regulated de-ice manifold. *(v2024.3.174)*
- Cabin heat is fed from bleed air through the duct, with the POH 25/20 PSI pressure protections. *(v2024.3.174)*
- Fixed the magnitude of engine performance penalties for bleed air and intake deflectors to be faithful to the POH. *(v2024.3.174)*

### Intake deflectors (inertial separator)

- Rewired to extend/retract on engine pressure and DC bus voltage, per POH. *(v2024.3.174)*
- Removed an incorrect torque/ITT penalty; the remaining effect is a small power reduction from the restricted inlet, per the POH power charts. *(v2024.3.174)*

### Anti-ice

- The **inertial separator** switch now holds in the Retract position for a moment after you let go. Before, it sometimes snapped back immediately without letting the switch animation play out, which looked broken and confused users. *(v2024.3.47)*
- **Windshield heat is now thermostat-controlled.** Instead of running flat-out whenever the switch is on, the window heat warms the glass and then cycles to hold temperature. The underlying thermal model is minimal, it should keep the windshield heat constantly on in flight in cool air, so it never interferes with the actual windshield icing effect (funny story, there is no way for the mod to know whether the windshield is iced over, this is purely a sim-internal effect which is not exposed anywhere, as far as I could see). The state of the thermostat and its modelled window temperature are visible on the debug tab. *(v2024.3.68)*
- Fix: In-air and runway starts have bleed air on. *(v2024.3.68)*

## L:vars

Variables worth binding to hardware, driving from FSUIPC / Axis&Ohs / SPAD.neXt,
or reading on an external display. **R** is read-only state; **R/W** can be set.
This is a selection, not the full list — the aircraft defines around a thousand
`DHC6_*` names, most of them internal.

| L:var | R/W | Meaning |
|---|---|---|
| `DHC6_DEICE_BOOTS` | R/W | De-ice boot cycle |
| `DHC6_DEICE_BOOTS_INFLATING` | R | Boots currently inflating — useful for an external annunciator |
| `DHC6_INTAKE_DEICE` | R/W | Intake de-ice |
| `DHC6_DEFLECTOR_CMD` | R/W | Intake deflector command |
| `DHC6_DEFLECTOR_POS_L` | R | Left deflector actual position — lags the command, needs bleed pressure |
| `DHC6_DEFLECTOR_POS_R` | R | Right deflector position |
| `DHC6_DEFLECTOR_EYE_L` | R | Left doll's-eye indicator |
| `DHC6_BLEED_PSI_DUCT` | R | Duct pressure |
| `DHC6_BLEED_PSI_MANIFOLD` | R | Manifold pressure |
| `DHC6_PNEUMATIC_LOW_PRESS` | R | Low pneumatic pressure warning |
| `DHC6_WINDOW_HEAT_TEMP_L` | R | Left windscreen temperature |
