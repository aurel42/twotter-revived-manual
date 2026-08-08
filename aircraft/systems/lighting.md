---
title: Lighting
review: draft
---

# Lighting

Every cockpit light has to pass four gates before it illuminates: the bus has to
be powered, the circuit's slot switch has to be closed, the potentiometer has to
be turned up, and the emissive itself has to be working. A lamp that stays dark
has failed one of those four, and they fail for different reasons.

## From the README

### Interior

- Battery terminal voltage now sags under heavy load. It's most visible during engine start — the voltmeter dips and the cockpit lighting (including the glareshield annunciators) dims as the starter pulls the bus down.
- Interior light dimming reacts to bus load.
- Annunciator brightness now uses a per-lamp aging factor.
- Added battery packs to emergency lighting. Armed emergency lights turn on when main power is lost and are powered for at least 15 minutes. (Limitation: the batteries are magically charged by the sim when the flight starts, trickle-charge is currently missing because of... erm... topological challenges.)
- Corrected emergency exit lights logic; they now illuminate correctly in the ARM position upon loss of power.
- Gated cockpit lighting and spoiler triggers to stop spamming events.

### Exterior

- Included hobanagerik's landing and taxi light tuning, with intensity brought up to MSFS 2024 values.

## From the release notes

Verbatim from `docs/release-notes-*.md`. The tag is the version each item first
appeared in.

### Interior

- [NEW] Interior light dimming reacts to bus load. *(v2024.2.x)*
- [NEW] Annunciator brightness now uses a per-lamp aging factor. *(v2024.2.x)*
- Added battery packs to emergency lighting. Armed emergency lights turn on when main power is lost and are powered for at least 15 minutes. *(v2024.2.x)*
- Corrected emergency exit lights logic; they now illuminate correctly in the ARM position upon loss of power. *(v2024.2.x)*
- Gated cockpit lighting and spoiler triggers to stop spamming events. *(v2024.2.x)*
- [NEW] Annunciator lights work again (regression introduced by SU5 release 1.7.21.0). *(v2024.2.x)*
- **Battery voltage now sags under heavy load.** Most visible during engine start — the voltmeter dips and the cockpit lighting dims as the starter pulls the bus down. *(v2024.3.68)*
- Each glareshield annunciator bulb now has independent age and cycle counts. Bulbs dim continuously as they age and can burn out. *(v2024.3.19)*
- Fixed the missing cockpit flashlight at night. *(v2024.3.183)*
- Fixed incorrect default lighting, avionics and system states across the spawn scenarios. *(v2024.3.183)*

### Exterior

- Landing and taxi lights: cones tuned by hobanagerik <3, based on reference images, with intensity brought up to MSFS 2024 values. *(v2024.3.183)*

## L:vars

For home-cockpit builders: **every light switch, not a selection.** Bind these
from FSUIPC, Axis&Ohs, SPAD.neXt or MobiFlight. **R** is read-only state; **R/W**
can be set.

### Exterior

| L:var | R/W | Meaning |
|---|---|---|
| `DHC6_LIGHT_LANDING_1` | R/W | Left landing light |
| `DHC6_LIGHT_LANDING_2` | R/W | Right landing light |
| `DHC6_LIGHT_TAXI` | R/W | Taxi light |
| `DHC6_LIGHT_NAV` | R/W | Navigation lights |
| `DHC6_LIGHT_BEACON` | R/W | Beacon |
| `DHC6_LIGHT_STROBE` | R/W | Strobes |
| `DHC6_LIGHT_WING` | R/W | Wing inspection light |

### Interior

| L:var | R/W | Meaning |
|---|---|---|
| `DHC6_COCKPIT_LT` | R/W | Cockpit lighting |
| `DHC6_UTIL_LTS` | R/W | Utility lights |
| `DHC6_UTIL_LT_CPT_RED` | R/W | Captain's utility light, red mode |
| `DHC6_UTIL_LT_FO_RED` | R/W | First officer's utility light, red mode |
| `DHC6_READING_LTS` | R/W | Reading lights |
| `DHC6_ENTRANCE_LTS` | R/W | Entrance lights |
| `DHC6_EMER_LT_STATE` | R/W | Emergency lighting: off / armed / on |
| `DHC6_SEAT_BELTS` | R/W | Seat-belt sign |
| `DHC6_ANNUN_LT_BRT` | R/W | Annunciator panel brightness |
| `DHC6_DIMMER_INT_FACTOR` | R | Interior dim factor the load model is applying — drops as the bus sags |
| `DHC6_DIMMER_EXT_FACTOR` | R | Exterior dim factor |
| `DHC6_DIMMER_ANN_FACTOR` | R | Annunciator dim factor, reconstructed from battery current |

## Input events (B:)

MSFS 2024 Input Events are the sim's own binding mechanism, and unlike L:vars they
are **enumerable** — MobiFlight, SPAD.neXt and Axis&Ohs discover them from the sim
without anyone publishing a list, and Dev Mode's Behaviors tab shows the one under
your cursor with `Ctrl`+`G`. That is why most aircraft publish L:var lists but not
Input Event lists.

The Twin Otter currently exposes only two lighting Input Events:

| Input event | Covers |
|---|---|
| `B:LIGHTING_IE_CEILING_LT_Set` | Ceiling light |
| `B:LIGHTING_STROBE_1`, `B:LIGHTING_STROBE_1_Set` | Strobes |

Everything else in the tables above is L:var only. For a hardware panel that is a
real gap, not a documentation gap — Input Events accept `_Set` / `_Inc` / `_Dec` /
`_Toggle` variants and are the better target for a physical switch.

Bulb ageing and burn-out are part of the
[Wear and tear](../../mod/features/wear-and-tear.md) system, not of lighting itself.
