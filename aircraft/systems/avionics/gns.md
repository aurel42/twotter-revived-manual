---
title: GNS 530/430
review: approved
---

# GNS 530/430

Credit where credit is due: the Working Title avionics are among the
best features of MSFS. The mod augments the GNS units not because of
limitations, but because they invite experimentation and
customization.

## Features

The mod unlocks two WT framework features that do not exist on
real-life GNS units, both can be disabled on the Config tab, but the
setting only becomes active on the next flight:

- "VISUAL" approaches
  Lifted from the G1000 NXi. The unit builds a VISUAL approach for any
  runway: a straight-in final on a 3° glidepath, listed alongside the
  published procedures. Select it and the autopilot flies it coupled,
  with no instrument procedure involved. No real GNS 530W or 430W
  offers this.

- Coupled VNAV
  Lifted from the G1000 NXi. The unit computes a descent path to the
  altitude constraints in your flight plan, and the autopilot flies it
  as PATH. Arm it from the VNAV page, or by loading a procedure that
  carries constraints. A real GNS 530W computes the same descent but
  only advises it.

## Persistence

The persistence is split into parts, dictated by legacy sim behavior:
frequencies are stored and restored as part of the opt-in persistence
system. The rest of the internal state of the units uses their own
persistence layer, independent of the mod. That means that the
internal state is fleet-wide instead of individual (per variant) and
stored and restored unconditionally and independent of the opt-in
system.

I added a few more items to the list, e.g. the state of the ADB-S
transmitter, the last selected page, and the selected range for each
page (needs validation vs. the real unit or the official Garmin
simulator).
