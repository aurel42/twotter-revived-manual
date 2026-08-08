---
title: Hydraulic system
review: draft
---

# Hydraulic system

## From the README

The hydraulic system now has a pump that draws power when it actuates. You can hear it work when you use the flaps, brakes, or nosewheel steering. It took a bit of trickery to work around sim limitations (or maybe dev limitations, can't be sure) and I haven't figured out how to prime the hydraulics, so every flight starts with an empty system and an invisible mechanic using the hand pump.

- Rebuilt hydraulic system using EX1.
- Added brake isolation valve to preserve some brake functionality on system failure.
- Added emergency hand pump to EFB Systems tab to manually restore hydraulic pressure and to prime the system when state is restored.
- Changed brake and sys pressure gauges to show actual values.
- Pump inrush: the hydraulic pump now pulls a brief power surge when it cuts in, before settling to its running draw — visible on the loadmeter.
- System pressure regulation and pressure-gauge calibration carry small per-variant offsets.

## From the release notes

Verbatim from `docs/release-notes-*.md`, grouped by the section they appeared in. The tag is the version each item first appeared in.

### [NEW] Hydraulic system

- Rebuilt hydraulic system using EX1. *(v2024.2.x)*
- Added brake isolation valve to preserve some brake functionality on system failure. *(v2024.2.x)*
- Added emergency hand pump to EFB Systems tab to manually restore hydraulic pressure and to prime the system when state is restored. *(v2024.2.x)*
- Changed brake and sys pressure gauges to show actual values. *(v2024.2.x)*

### Cockpit & systems

- Fixed rudder pedal axis jitter driving the nose wheel steering actuator, triggering a ridiculous amount of hydraulic pump runs. *(v2024.3.183)*

### Hydraulics

- **Pump inrush.** The hydraulic pump now pulls a brief power surge when it cuts in before settling to its running draw, visible on the loadmeter. *(v2024.3.68)*
- As part of the ongoing work to add individualization to airframes, the system pressure regulation and the pressure-gauge calibration now carry small per-variant offsets. *(v2024.3.68)*
- **The pump cycles less often** during routine flap, brake and nose-wheel-steering use (actuator volumes retuned). Feedback is welcome. (Still too frequently?) *(v2024.3.68)*

## L:vars

Variables worth binding to hardware, driving from FSUIPC / Axis&Ohs / SPAD.neXt,
or reading on an external display. **R** is read-only state; **R/W** can be set.
This is a selection, not the full list — the aircraft defines around a thousand
`DHC6_*` names, most of them internal.

| L:var | R/W | Meaning |
|---|---|---|
| `DHC6_HYD_PUMP_RUN` | R | Hydraulic pump currently running — the cycling you hear |
| `DHC6_HAND_PUMP` | R/W | Emergency hand pump. **No cockpit control** — EFB Systems tab or a binding only |
