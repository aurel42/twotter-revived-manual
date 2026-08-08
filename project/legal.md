---
title: Legal
review: approved
---

# Legal

## Disclaimer

Twotter Revived is a non-commercial fan project, not affiliated with,
endorsed by, supported by, or even acknowledged by Microsoft, Asobo,
or Aerosoft. We don't accept monetary donations, but we're happy to
accept code or assets for use in the mod.

Twotter Revived is not endorsed or certified by De Havilland Aircraft
of Canada.

Twotter Revived is not approved by anyone in any way, shape, or form
for training or any other real-world application. Any real-world
references are provided for entertainment and educational simulation
use only and may be incomplete, outdated, configuration-specific, or
inaccurate.

## The base aircraft

The base aircraft is **not** redistributed by this project. The mod
ships as binary deltas that are applied to your own copy of the
Aerosoft Twin Otter v1.1.1 on your own machine. The original add-on
remains the property of its rights holder: Aerosoft or Microsoft; the
ownership situation is genuinely unclear.

The practical consequence: **the assembled mod cannot be
redistributed.** Once the installer has run, the folder on your disk
contains reconstructed derivatives of files you licensed from
Aerosoft. Only the unassembled form — the `.xdelta` patches plus our
own new files — may be distributed.

Modified sim assets are used under Microsoft's [Game Content Usage
Rules](https://www.xbox.com/developers/rules) and Working Title's
MIT-like license.

## Our own code

The complete project source cannot be published, because CFG and XML
files still contain fragments of the base aircraft. What is published
is the WASM module source, which is the heart of the mod.

The custom code written for this project, other than the bundled
components listed below, is placed in the **public domain**. Take it
apart, use it, modify it, redistribute it.

I only ask that you report bugs so I can fix them for Twotter
Revived. This is just a polite request, not a legal requirement.

## Licenses

Third-party code included in, or shipped alongside, the mod.

### libradio

- **License:** [CDDL-1.0](https://opensource.org/license/cddl-1-0) — SPDX
  `CDDL-1.0` — covering **the parts of libradio we ship** (see below), not the
  underlying ITM code
- **Attribution:** the thread-safety modification present in these files is
  **Saso Kiselkov's** contribution
- **Obtained from:** [libradio](https://github.com/skiselkov/libradio) by Saso
  Kiselkov, vendored byte-identical and unmodified
- **Underlying algorithm:** created by the U.S. Department of Commerce
  **NTIA/ITS**, Institute for Telecommunication Sciences

The two files we ship carry an NTIA banner and **no copyright notice
or license block of any kind**. That is deliberate upstream: libradio
has no repository-level license.

Our own glue is separate and not derived from those files: the bridge
wrapper is an independent reimplementation of the shim libradio ships
as CDDL-headered code, and the surface-constants table was
reimplemented from ITU-R P.527 rather than copied.

### puff — DEFLATE inflate

Decompresses the terrain tiles, so the WASM module needs no zlib at runtime.

- **License:** [zlib](https://opensource.org/license/zlib) — SPDX `Zlib`
- **Copyright:** © 2002–2013 Mark Adler, all rights reserved (version 2.3, 21 Jan 2013)
- **Upstream:** [zlib](https://github.com/madler/zlib), `contrib/puff`

Clause 2 of the license requires that altered versions be plainly marked, so:
**this copy is altered.** The `<setjmp.h>` dependency was removed — the MSFS wasm
sysroot ships no `setjmp.h` — and the original `setjmp`/`longjmp` "out of input"
escape was replaced with an overrun flag and an error return. The inflate
algorithm and its output are unchanged, and the changes are marked in the source.

### Working Title GNS 430/530

The GNS 530 and GNS 430 units in the panel. Vendored and patched in-tree.

- **License:** MIT with a field-of-use restriction —
  [full text](https://github.com/microsoft/msfs-avionics-mirror/blob/main/LICENSE).
- **Copyright:** Microsoft Corporation. Authored by Working Title Simulations.
- **Upstream:** [msfs-avionics-mirror](https://github.com/microsoft/msfs-avionics-mirror/)

The restriction is the part that matters: it grants the usual MIT
permissions — use, modify, distribute, sublicense, sell — but only for
use *within Microsoft Flight Simulator*. Licensing for anything else
is prohibited.

The GNS bundle also carries `msfssdk.js` and `garminsdk.js`, built
from `@microsoft/msfs-sdk` and `@microsoft/msfs-garminsdk`, under the
same license.

### xdelta3

Reconstructs the modified aircraft files on your machine during
installation.  Shipped as `xdelta3.exe` in the mod folder — an
unmodified upstream binary, distributed alongside our work rather than
linked into it.

- **License:** [GPL-2.0-or-later](https://www.gnu.org/licenses/old-licenses/gpl-2.0.html)
  — SPDX `GPL-2.0-or-later`
- **Copyright:** Josh MacDonald
- **Upstream:** [xdelta](https://github.com/jmacd/xdelta-devel)

## Bundled audio

### FlyByWire Simulations — CC BY-NC-SA 4.0

The `fbw_egpws_*` callout files and `fbw_flap_actuator.wav` derive from
FlyByWire Simulations' [a32nx-wwise](https://github.com/flybywiresim/a32nx-wwise),
under **Creative Commons Attribution-NonCommercial-ShareAlike 4.0**.

Most modifications are pure format conversions to 16-bit mono PCM; the
terrain, glideslope and terrain-ahead clips were additionally cut by
hand from a longer source clip. All remain CC BY-NC-SA 4.0.

Two obligations follow, and they attach to the mod as distributed, not merely to
the files:

- **NonCommercial.** This mod may not be sold or distributed commercially.
- **ShareAlike.** Derivatives of those audio files carry the same license.

### Black Square / Boris Audio Works

The optional Turbine Duke sound mod re-uses the Turbine Duke's Wwise
soundbank with kind permission from Black Square and Boris Audio
Works. You must own the Black Square Turbine Duke, no bank content is
redistributed.

### Project-owned callouts

The custom TAWS bank and the copilot checklist responses, were
generated by a commercial TTS service for this project and since no
person was involved in creating them, nobody can claim any rights for
them. Whether it was ethical to _base_ them on the voice of a person
is beyond my paygrade. If anyone feels offended, let me know.

## Bundled models

### Ground power unit — CC BY 4.0

The GPU cart that appears beside the aircraft is
["GPU (Aiport Ground Power Unit)"](https://sketchfab.com/3d-models/gpu-aiport-ground-power-unit-b18e62dda2e04e97a95312bb6a284f1c)
by **omri_ha_muglob**, under **Creative Commons Attribution 4.0**. Bagolu fitted
it to the Twin Otter.

The wheel chocks are Bagolu's own model, donated to the project.

## Data

| Source | License |
|---|---|
| **ETOPO 2022** 60″ global surface DEM (NOAA NCEI) — terrain elevation | U.S. Government work, public domain |
| **Natural Earth** 1:10m ocean / lakes / glaciated areas — coastlines, water, ice | public domain |
| **ESA WorldCover 2021 v200** — land cover | **CC BY 4.0**, attribution required |
| **OurAirports** `navaids.csv` — navaid coverage | public domain |
| **FlightGear** `nav.dat` — navaid service ranges, historic NDB database | **GPL-2.0-or-later** |

ESA WorldCover: © ESA WorldCover project / contributors.

FlightGear's `nav.dat` originates in the X-Plane navdata compiled by **Robin A.
Peel**. The shipped artifacts are derived data — service-range tables and the
historic NDB database — baked at build time; the GPL source itself is not
redistributed and the WASM build does not depend on it.

## In practice

If you are redistributing or building on this project:

- The mod as distributed is **NonCommercial**, because of the FlyByWire audio.
- The assembled mod folder **cannot** be redistributed at all — it contains
  reconstructed Aerosoft-derived files.
- Attribution is required for ESA WorldCover, for the FlyByWire audio, and for the
  ground power unit model.
- CDDL and GPL components carry source-availability obligations, met by the
  published WASM source and by xdelta3 being an unmodified upstream binary.
