---
title: Cabin climate
review: approved
---

# Cabin climate

## Overview

The Cabin Climate system exists for two reasons: for flavor and as a
consumer of the heating system which is driven by bleed air.

It's a low-fidelity three-zone thermal model (cockpit / cabin / aft
cabin) with air and structure temperatures for the zones. 

Heat comes from the heating system (driven by engine bleed air),
avionics, passenger body heat and a directional solar greenhouse
effect (sun geometry × heading, mitigated by cloud cover).

Cooling is provided by by ram air (unless you're moving, this needs
the vent fan), radiation to the sky, and, if ambient conditions allow,
by opening the doors on the ground (ideally opposed doors in a
breeze). The cockpit fans (missing in the visual model) help with the
heat exchange between cockpit air and cockpit structures, they can
help cool the avionics.

On a cold-and-dark spawn the conditions are based on the assumption
that the empty aircraft been sitting on the apron for a few hours by
replaying the preceding hours of sun position. As a simplification,
the model assumes the weather didn't change during that time.

## Air quality

A cabin air model tracks CO₂, O₂ and humidity, shown on the EFB
Climate tab alongside the three zone temperatures and the duct. This
is purely for flavor and to provide some incentive to let fresh air
in.

## L:vars

| L:var | R/W | Meaning |
|---|---|---|
| `DHC6_CABIN_TEMP_COCKPIT` | R | Cockpit zone air temperature |
| `DHC6_CABIN_TEMP_MID` | R | Mid-cabin zone |
| `DHC6_CABIN_TEMP_AFT` | R | Aft-cabin zone |
| `DHC6_CABIN_DUCT_TEMP` | R | Bleed-air duct temperature |
| `DHC6_DUCT_OVERHEAT` | R | Duct overheat flag |
| `DHC6_CLIMATE_MODE` | R/W | Manual / Off / Auto |
| `DHC6_CLIMATE_MANUAL` | R/W | Manual COOL / Hold / WARM |
| `DHC6_CLIMATE_TEMP_SET` | R/W | Temperature selector |
| `DHC6_CABIN_VENT_FAN` | R/W | Vent fan — a cockpit switch exists, but worth a hardware binding |
| `DHC6_COMPT_FANS` | R/W | Compartment fans |
