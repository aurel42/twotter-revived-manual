---
title: Instruments
review: approved
---

# Instruments

## Overview

Work in progress: many gauges are getting an overhaul, giving them
some physicality and changing their behavior, based on reference
videos, manuals, pilot feedback and filling in the gaps with educated
guesses until I get better references.

> [!NOTE]
> Some gauges have small calibration issues. Your mechanic tells you
> that's fine, don't worry about it.

All the gyros are simulated individually, if you can't appreciate the
concert they play for you during spool-up, use the Headset button. All
gyros are electrical, their load is simulated. Our Twotter doesn't
need or have a vacuum system.

## HSI

The Horizontal Situation Indicator (HS) is based on the Bendix/King KI 525A.

- Gyro: CPT slaved at 180°/min; FO free-DG with drift. Chaos-walk precession while spooling.
- Sync — EFB "Sync Gyros" is standing in for the KA-51B CW/CCW procedure.
- Course pointer — 30°/s slew, referenced to the gyro card. FO references the CPT value.
- Flags — NAV on signal/receiver state, GPS-blanked. HDG on avionics, rotor < 0.95, or lost slaving 26V. Independent paths, so HDG fail leaves the nav display live.
- AP disconnect on HDG-flag conditions or during sync.
- KI-227 ADF cards slaved to per-side gyro.

> [!CAUTION]
> **TODO**
>
> The Slaving Control unit is not yet implemented.

## Altimeters

The captain's is based on an Intercontinental Dynamic Corporation
Encoding Altimeter (28V), a servo-driven encoding unit, not a plain
aneroid. Everything that moves on the face is driven by the motor, the
needle as well as the drums. The first officer's is a plain aneroid,
connected to static and needing no power at all.

The drums are geared. They sit locked on their current digit until the
hundreds drum passes 920, then roll through to the next in the last
eighty feet. The ten-thousands drum only moves in that same
eighty-foot window before each ten thousand.

Cut power to the instrument and it freezes with everything where it
stood. The Kollsman scale still turns and moves the needle, because
that linkage is mechanical.

## Whiskey Compass

The stock sim compass is pretty much perfect. All I added to it was a
deflection when Windshield Heat is in use and the thermostat turns on
the heating elements. The amplitude of the deflection depends on the
aircraft's magnetic heading and attitude.

## Radio Altimeter

- Equipped with a highly reponsive servo.

## L:vars

**R** is read-only state; **R/W** can be set.

| L:var | R/W | Meaning |
|---|---|---|
| `DHC6_GYRO_SYNC_REQUEST` | R/W | Sync the HSI card to the whiskey compass. **No cockpit control** — EFB or binding only |
| `DHC6_GYRO_CPT_HEADING` | R | Captain's gyro heading, drift included |
| `DHC6_GYRO_FO_HEADING` | R | First officer's gyro heading |
| `DHC6_GYRO_CPT_OMEGA` | R | Captain's gyro rotor speed |
| `DHC6_ADF_NEEDLE_CPT` | R | Captain's ADF needle bearing |
| `DHC6_ADF_CARD_CPT_OFFSET` | R | ADF card offset from the slaved heading |
| `DHC6_COMPASS_DEV_OFFSET` | R | Whiskey compass deviation |
| `DHC6_ALT_ALERT` | R | Altitude alert state |
| `DHC6_ALT_ALERT_LIGHT` | R | Altitude alert lamp |
