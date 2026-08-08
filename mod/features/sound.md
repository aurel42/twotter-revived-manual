---
title: Sound
review: approved
---

# Sound

## Overview

The audio of the mod consists of several layers.

- The base is the Wwise soundback that comes with your Aerosoft Twin
  Otter. It contains many standard sounds, but also typical Twin Otter
  sounds like the whistle of the hydraulic pump.
  It's not authored very well compared to more modern Wwise banks.

- The the Aerosoft Wwise soundbank is augmented by a few wav files
  shipping directly with the mod, for example an interior flaps sound
  that matches the continuous movement of the flaps better than the
  default sounds.

- I created a bunch of callouts using fish.audio to close gaps. All
  except two autopilot trim warnings can be muted or disabled.

    - A complete set of TAWS callouts.

    - A set of KAP 140 callouts, based only on mentions in the manual,
      I could not find any reference material.

    - A set of co-pilot responses to checklist items.

- The distributed archive includes an experimental, separate mod that
  allows owners of the Black Square Turbine Duke to re-use its
  excellent Wwise soundback with the Twotter.

  The Headset button on the Systems tab only works for sounds from
  this sound bank and for custom sounds. There is no way (that I know
  of) to control the volume of events coming from the Aerosoft Wwise
  soundbank.

## Known issues

- Default audio (without the Duke mod) isn't very good.

- Amphibious and float variants have water effects that trigger when
  they shouldn't. Work in progress.
## L:vars

Variables worth binding to hardware, driving from FSUIPC / Axis&Ohs / SPAD.neXt,
or reading on an external display. **R** is read-only state; **R/W** can be set.
This is a selection, not the full list — the aircraft defines around a thousand
`DHC6_*` names, most of them internal.

| L:var | R/W | Meaning |
|---|---|---|
| `DHC6_HEADSET` | R/W | Headset attenuation (0..1), 0 is off, 1 mutes the sound completely. The EFB button sets 0.6, a binding can write any value in the range. |
| `DHC6_RAM_AIR` | R/W | RAM air lever (0..100), 0 is closed, 100 is fully open. |
