---
title: Radio navigation
review: draft
---

# Radio navigation

## From the README

The goal is: replace all navaid signals with a physics- and terrain-based simulation.
State: Mostly complete. The custom signal now drives the steam gauges and the autopilot directors.

All radio navaid signals are now simulated in custom code. All of the needles, TAWS and AP are driven by signal-strength-first propagation models. Stations fade, drop out behind ridges, interfere with each other, and the needles wander, jitter and lag. And there's an EFB tab to show you why.

Note: At this point, ONLY the steam gauges are migrated to the new system. AP and GNS units are still using stock sim data (but not for very much longer). If the needles show a slight deviation while you're on AP, that's expected. If the needles do one thing and the AP does a totally different thing, that's possibly a bug that should be reported, ideally with video or screenshots of the Radio tab.

**UHF/VHF: VOR, ILS localizer & glideslope, DME** (line-of-sight bands):
- Free-space path loss: inverse-square spreading on the true slant range (Friis free-space formula).
- Terrain diffraction & shadowing: the Longley-Rice Irregular Terrain Model (the NTIA/ITS ITM), run against a baked global terrain and land-cover dataset (≈1.85 km cells, derived from ESA WorldCover land cover and Natural Earth water/ice).
- DME on its real frequency: DME is paired to its true UHF / L-band channel (ICAO Annex 10) and run through a separate terrain pass, so it digs a deeper shadow behind a ridge than the co-located VOR.
- Service volume & antenna pattern: per-class range and elevation/cone roll-off curves (ICAO service volumes; libradio curves).
- Co-channel interference (12 dB capture): of all stations on the tuned frequency, the receiver locks only the strongest, and only if it leads the runner-up by ≥ 12 dB; inside that margin it garbles to a dropout (ICAO capture ratio).
- Radial & CDI geometry: the VOR radial and course deviation are built from the station's position, its commissioned magnetic variation, and your OBS course (ICAO Annex 10).
- Per-runway localizer width: the on-course sector width is derived from the actual runway geometry, clamped to the Annex-10 bounds.
- False glideslopes: capture lobes at odd multiples of the true GS angle, falling out of the antenna geometry (ICAO Annex 10).
- Independent glideslope signal: GS strength is computed separately from the localizer.
- Signal-dependent needle lag: the weaker the signal, the heavier the servo smoothing and the more the needle lags (libradio lag law).
- Usable / identifiable floor (hysteretic): usable at −70 dB, can't-ID below −73 dB; below the floor the CDI parks to centre, the GS retracts, TO/FROM reads OFF and the NAV flag drops (libradio dB thresholds).
- DME receiver behaviour: slant range, an overhead no-track zone, memory/hold on signal loss, and a line-of-sight horizon range limit (modelled on the KN-62).

**LF/MF: NDB** (ground-wave + night sky-wave):
- Ground-wave field strength: ITU-R Recommendation P.368 ground-wave curves (1 kW vertical-monopole reference) over detailed ground regimes based on nine terrain types plus permafrost and wet tundra.
- Mixed land–sea paths: Millington's method (P.368 Annex 2); surface conductivity comes from the baked land-cover/water dataset.
- Transmit power by class: NDB power scaled per service class.
- Height gain: an elevated-receiver correction, since the surface-only P.368 curves under-read for an aircraft at altitude.
- LF terrain masking: a gentle knife-edge loss (3–15 dB, vs VHF's 20–60), because long LF wavelengths bend around terrain.
- Night sky-wave range extension: ITU-R Recommendation P.1147 single-hop E-layer night field, power-summed with the ground wave.
- Diurnal variation: the P.1147 hourly absorption factor: sky-wave is crushed by day and deepest in the middle of the night.
- Seasonal variation: a P.1147-style seasonal term (MF semi-annual / LF annual, scaled by frequency and latitude, hemisphere-aware).
- Geomagnetic-latitude absorption: the dominant night-field absorption scales with geomagnetic latitude (centered-dipole model).
- Coastal refraction: bearings bend where the signal path crosses a coastline at a shallow angle (P.368 coastal effect).
- Quadrantal error: airframe re-radiation distorts the bearing as a per-airframe sine series (FAA Order 6740.6).
- Bank / dip error: falls out of a full 3-D line-to-station projection: a bank tilts the loop plane and the needle errs, worst at the fore/aft headings.
- NDB cone of silence: a narrow overhead cone (~0.2 NM radius at 10,000 ft) where the needle wanders.
- Night-effect bearing swing: twilight ionospheric fluctuation, gated by solar elevation, that makes the needle swing around dawn and dusk.
- Near-field multipath: a slowly-rotating reflection ghost whose near-field zone scales with the tuned wavelength.
- Deterministic, signal-driven bearing error.
- Curated historical NDB database (~9k beacons) with baked class/range and first/last-seen years.
- Offline NDB classification: station class/range derived from location + terrain heuristics.
- Terrain relief & urban bending, coastal-refraction overhaul, noise floor varying with geography/time/season, and co-channel stations combining as a field-weighted resultant bearing.

## From the release notes

Verbatim from `docs/release-notes-*.md`, grouped by the section they appeared in. The tag is the version each item first appeared in.

### Instruments — gyros, attitude, HSI, ADF, NAV

- VOR cone-of-confusion modelled. Work in progress. Disabled in this build, because it doesn't look realistic yet. *(v2024.2.134)*

### Radio navigation

- Fixed standalone DME and TACAN stations incorrectly deflecting the CDI. *(v2024.3.130)*
- Toned down overdone jitter of localizer signals. *(v2024.3.130)*
- Mitigated spurious terrain occlusion affecting localizers, caused by limitations of the terrain data. *(v2024.3.130)*
- Fixed the glideslope indicator parking on some precision approaches. *(v2024.3.130)*
- Free-space path loss: inverse-square spreading on the true slant range (Friis free-space formula). *(v2024.3.99)*
- Terrain diffraction & shadowing: the Longley-Rice Irregular Terrain Model (the NTIA/ITS ITM), run against a baked global terrain and land-cover dataset (≈1.85 km cells, derived from ESA WorldCover land cover and Natural Earth water/ice). *(v2024.3.99)*
- DME on its real frequency: DME is paired to its true UHF / L-band channel (ICAO Annex 10) and run through a separate terrain pass, so it digs a deeper shadow behind a ridge than the co-located VOR. *(v2024.3.99)*
- Service volume & antenna pattern: per-class range and elevation/cone roll-off curves (ICAO service volumes; libradio curves). *(v2024.3.99)*
- Co-channel interference (12 dB capture): of all stations on the tuned frequency, the receiver locks only the strongest, and only if it leads the runner-up by ≥ 12 dB; inside that margin it garbles to a dropout (ICAO capture ratio). *(v2024.3.99)*
- Radial & CDI geometry: the VOR radial and course deviation are built from the station's position, its commissioned magnetic variation, and your OBS course (ICAO Annex 10). *(v2024.3.99)*
- Per-runway localizer width: the on-course sector width is derived from the actual runway geometry, clamped to the Annex-10 bounds. *(v2024.3.99)*
- False glideslopes: capture lobes at odd multiples of the true GS angle, falling out of the antenna geometry (ICAO Annex 10). *(v2024.3.99)*
- Independent glideslope signal: GS strength is computed separately from the localizer. *(v2024.3.99)*
- Signal-dependent needle lag: the weaker the signal, the heavier the servo smoothing and the more the needle lags (libradio lag law). *(v2024.3.99)*
- Usable / identifiable floor (hysteretic): usable at −70 dB, can't-ID below −73 dB; below the floor the CDI parks to centre, the GS retracts, TO/FROM reads OFF and the NAV flag drops (libradio dB thresholds). *(v2024.3.99)*
- DME receiver behaviour: slant range, an overhead no-track zone, memory/hold on signal loss, and a line-of-sight horizon range limit (modelled on the KN-62). *(v2024.3.99)*
- Ground-wave field strength: ITU-R Recommendation P.368 ground-wave curves (1 kW vertical-monopole reference) over four ground regimes (sea / wet ground / good land / poor land). *(v2024.3.99)*
- Mixed land–sea paths: Millington's method (P.368 Annex 2); surface conductivity comes from the baked land-cover/water dataset. *(v2024.3.99)*
- Transmit power by class: NDB power scaled per service class. *(v2024.3.99)*
- Height gain: an elevated-receiver correction, since the surface-only P.368 curves under-read for an aircraft at altitude. *(v2024.3.99)*
- LF terrain masking: a gentle knife-edge loss (3–15 dB, vs VHF's 20–60), because long LF wavelengths bend around terrain. *(v2024.3.99)*
- Night sky-wave range extension: ITU-R Recommendation P.1147 single-hop E-layer night field, power-summed with the ground wave. *(v2024.3.99)*
- Diurnal variation: the P.1147 hourly absorption factor: sky-wave is crushed by day and deepest in the middle of the night. *(v2024.3.99)*
- Seasonal variation: a P.1147-style seasonal term (MF semi-annual / LF annual, scaled by frequency and latitude, hemisphere-aware). *(v2024.3.99)*
- Geomagnetic-latitude absorption: the dominant night-field absorption scales with geomagnetic latitude (centered-dipole model). *(v2024.3.99)*
- Coastal refraction: bearings bend where the signal path crosses a coastline at a shallow angle (P.368 coastal effect). *(v2024.3.99)*
- Quadrantal error: airframe re-radiation distorts the bearing as a per-airframe sine series (FAA Order 6740.6). *(v2024.3.99)*
- Bank / dip error: falls out of a full 3-D line-to-station projection: a bank tilts the loop plane and the needle errs, worst at the fore/aft headings. *(v2024.3.99)*
- NDB cone of silence: a narrow overhead cone (~0.2 NM radius at 10,000 ft) where the needle wanders. *(v2024.3.99)*
- Night-effect bearing swing: twilight ionospheric fluctuation, gated by solar elevation, that makes the needle swing around dawn and dusk. *(v2024.3.99)*
- Near-field multipath: a slowly-rotating reflection ghost whose near-field zone scales with the tuned wavelength. *(v2024.3.99)*

### DME

- Added a KDI-572-inspired readout on the EFB app Systems tab with station ident, frequency, slant distance, DME ground speed, time-to-station. *(v2024.3.99)*
- "HOLD" locks the current station so you can re-tune your NAV radios without changing the DME. *(v2024.3.99)*
