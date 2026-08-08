---
title: EGPWS
review: draft
---

# EGPWS

## From the README

- Overhauled EGPWS suite based on Honeywell Pilot's Guide and FAA advisories.
- EGPWS callouts are now configurable via the **TAWS** selector on the EFB Config tab.
- EPGWS callouts come in selectable soundbanks (Aerosoft, FlyByWire, Custom) with varying coverage. Only Custom has the complete set of callouts.
- Declutter governor: an advisory is only repeated if the situation degrades.
- The GPWS INHIBIT button suppresses some callouts and desensitizes others.
- The EFB has a TAWS tab that shows per-mode margin bars and Honeywell-style envelope plots.
- EGPWS terrain warnings are now driven by the custom radio-signal / propagation model.

## From the release notes

Verbatim from `docs/release-notes-*.md`, grouped by the section they appeared in. The tag is the version each item first appeared in.

### Terrain awareness (EGPWS)

- Terrain warnings are now driven by the same custom signal-propagation model as everything else. *(v2024.3.124)*

### Terrain awareness (EGPWS / TAWS)

- **New "TAWS" selector on the EFB Config tab (Avionics section).** *(v2024.3.68)*
- **Full** — every callout on every variant (terrain, sink rate, glideslope, bank angle, the full altitude-callout ladder, MINIMUMS). *(v2024.3.68)*
- **Typical** — passenger variants get the full set; cargo/utility variants get the reduced Class B set. *(v2024.3.68)*
- **Legal minimum** — passenger variants only; cargo/utility variants get nothing. *(v2024.3.68)*
- **Class B** — the reduced TSO-C151 Class B set everywhere (excessive descent rate, altitude loss after takeoff, the "500 feet" callout, and MINIMUMS — no terrain, glideslope or bank-angle callouts). *(v2024.3.68)*
- **Off** — all callouts muted. *(v2024.3.68)*
- **No more spurious callouts in normal descents.** Reworked the flight-phase detection so a normal powered descent no longer trips "too low terrain" / "too low flaps" when you aren't configured for landing. *(v2024.3.68)*
- **Tighter glideslope logic.** The glideslope callout now requires a centered localizer on the front course. *(v2024.3.68)*
- Implemented declutter governor so advisories are only repeated if the situation degrades. *(v2024.3.68)*
- The GPWS INHIBIT button is now a shortcut for "turn everything off or down that can be legally turned off or down". It suppresses some callouts and desensitizes others. As I understand it, the real-world unit would have several switches to disable specific features, our implementation bunches them all together in this button. *(v2024.3.68)*
- Known issues: forward-looking terrain alerting is not yet implemented, but we have a reference implementation in Working Title's EPIC. *(v2024.3.68)*
- Added Forward-Looking Terrain Alerting (FLTA). *(v2024.3.99)*
- Added Premature Descent Alert (PDA). *(v2024.3.99)*
- Fixed "Smart 500". *(v2024.3.99)*
- Added missing altitude callouts for 1000 and 200 ft, and "approaching minimums." *(v2024.3.99)*
- **Choice of voices.** New TAWS soundbank selector on the EFB Config tab: the original **Aerosoft** Wwise callouts (the default), a **FlyByWire** voice set, or a **Custom** set. Not all soundbanks support all callouts. Switch to the Custom soundbank to hear the full set. *(v2024.3.99)*
- Reworked terrain-closure (Mode 2) onto a proper closure-rate-versus-height envelope. *(v2024.3.99)*
- Fixed a window just after rotation where "too low terrain" could nuisance-fire over rising ground on climb-out. *(v2024.3.99)*
- Added EFB TAWS tab exposing each mode's margin to its alerting boundary. Somewhat hidden: click the bar to show Honeywell-style envelope plots. *(v2024.3.99)*

## Config options

Set from the EFB **Config** tab. Each option is documented on exactly one page; see [Configuration](../../mod/configuration.md) for how profiles and the per-variant override work.

### Installation

`DHC6_CFG_GPWS` — default: **Full**

Terrain Awareness & Warning System fit — Full / Reduced / Legal minimum / Class B / Off. Selects which callouts the aircraft is equipped for, per variant.

### Sound bank

`DHC6_CFG_GPWS_AUDIOBANK` — default: **Aerosoft**

Which voice set plays: Aerosoft (the original bank), FlyByWire, or Custom (project-owned). See [Legal](../../project/legal.md) for the FlyByWire licence.

### Volume

`DHC6_CFG_GPWS_VOLUME` — default: **0 dB**

Callout volume, in ten detents from Off to 0 dB.
