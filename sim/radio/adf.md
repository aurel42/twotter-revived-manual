---
title: ADF and the KR 87
review: draft
---

# ADF and the KR 87

> [!CAUTION]
> **Placeholder**
>
> This page exists to demonstrate a never-approved page in the PoC. It has
> real content but is deliberately thin, and it should not appear in the
> published manual.

## Operation

**The needle is not a pointer, it is a measurement.** Bearing accuracy degrades
with signal strength rather than switching between "works" and "does not work",
so a distant or weak NDB gives you a needle that wanders around the true bearing
instead of one that suddenly flags. Close in, it is steady.

**Historic beacons are available.** *(Config: **Enable historic NDBs**.)* Modern
navdata has dropped most of the beacons that made low-altitude navigation
interesting; the option restores decommissioned stations from our own database.
Frequencies below 160 kHz with no gameplay relevance are excluded.

## Under the hood

An NDB is a vertically polarised transmitter whose ground wave follows terrain and
whose sky wave arrives after dark by a different path. The receiver cannot tell
them apart, which is the physical basis of night effect.

We drive needle accuracy from a signal-strength model anchored on ICAO rated
coverage — antenna efficiency, station class and field-strength contours — rather
than a distance cutoff. The bearing error added on top is deterministic and keyed
to that signal level, so the same station at the same range behaves the same way
every time.

Not modelled: lightning. The sim does not expose the data that would be needed to
bend the needle toward a storm cell.

## From the release notes

Verbatim from `docs/release-notes-*.md`, grouped by the section they appeared in. The tag is the version each item first appeared in.

### Instruments — gyros, attitude, HSI, ADF, NAV

- ADF noise model rewritten. *(v2024.2.134)*
- KI 227 ADF card now slaves to the gyro. Default behaviour: card follows `DHC6_GYRO_{CPT,FO}_HEADING`; the HDG knob nudges the gyro card directly (KI-227-01). New EFB option **"Disable ADF Card Slaving"** reverts to the legacy KI-227-00 manual-knob path. *(v2024.2.134)*

### Cockpit instruments

- [NEW] Added needle and radio physics to the ADF gauge. Based on KI 227. *(v2024.2.x)*

### Bug fixes

- Updated ADF circuit to 31 to match standard electrical configurations. *(v2024.2.x)*

### ADF / NDB realism

- **Historical NDB database.** Added a curated database of historical NDB beacons, with a year-scrubber panel on the EFB Radio tab to filter beacons to those active in a given year. Stations are auto-classified (class and range) from their location and surrounding terrain. *(v2024.3.124)*
- **Propagation overhaul.** Reworked LF/MF ground-wave and night sky-wave propagation: terrain relief and signal bending around mountains, urban multipath, coastal refraction, atmospheric noise, smoother sky-wave transitions around sunrise/sunset, and dedicated ground types for permafrost and wet tundra. *(v2024.3.124)*
- Fixes: signals receivable well beyond their realistic range, daytime sky-wave incorrectly extending range. *(v2024.3.124)*

### ADF realism overhaul (with input from CCM)

- **Bank/dip error.** Overfly a station in a turn and the needle leans toward the low wing. This falls straight out of the geometry (the loop antenna tilts with the airframe). *(v2024.3.47)*
- **Quadrantal error.** Each airframe carries its own small bearing distortion off the cardinal headings, rolled per session — so no two Twotters read quite identically. *(v2024.3.47)*
- In a tight cone directly overhead the station, multipath interference confuses the receiver. *(v2024.3.47)*
- **Distance and weak signal.** More wander the farther out you are; on a fading signal the needle twitches, and on a sustained loss it parks to 90° ("back to the peg"). *(v2024.3.47)*
- **Night and dusk.** Extra skywave wander after dark, driven by the position of the sun. *(v2024.3.47)*
- **Per-station character.** Each NDB feels a little different and drifts gently within a session. This RNG-driven effect is not grounded in physics as such, it stands in for actual effects we cannot simulate because of sim limitations (like terrain, coastal refraction, weather etc.). *(v2024.3.47)*
- **Real station altitude.** The cone now uses the tuned NDB's actual elevation from the nav database, so a mountain-top beacon behaves differently from a sea-level one. *(v2024.3.47)*
- ANT mode parks the needle (per KR 87 manual). *(v2024.3.47)*
- Needle freezes in position on receiver power loss. No magical parking without the receiver driving it. *(v2024.3.47)*
- Fixed: frequency and modes can no longer be changed while the receiver is off or unpowered. *(v2024.3.47)*
- Known issues: the system cannot react to lightning, the sim doesn't provide the necessary data. *(v2024.3.47)*

### The KR-87 ADF unit

- ADF frequencies restore more reliably. *(v2024.3.99)*

## Config options

Set from the EFB **Config** tab. Each option is documented on exactly one page; see [Configuration](../../mod/configuration.md) for how profiles and the per-variant override work.

### Enable historic NDBs

`DHC6_CFG_NDB_SOURCE` — default: **off**

Adds historic NDB beacons that modern navdata has dropped. When on, these take precedence over the sim's navdata; the sim is used only for NDBs not in the historic set.

### (historic NDB vintage)

`DHC6_CFG_NDB_YEAR` — default: **—**

Selects which year's beacon set the historic NDB database presents. Set from the EFB alongside the option above rather than being a switch of its own.

### Disable ADF Card Slaving

`DHC6_CFG_DISABLE_ADF_CARD_SLAVE` — default: **off**

Stops the ADF compass card from following the HSI heading.
