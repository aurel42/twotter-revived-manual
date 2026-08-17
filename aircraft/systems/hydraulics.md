---
title: Hydraulics
review: approved
---

# Hydraulic system

The hydraulic system powers the flaps, nose-wheel steering, and toe
brakes from a single electrical pump (running off the left DC bus)
that cycles on and off via a pressure switch.  The pump engages
whenever the pressure in the main accumulator falls below ca. 1350
PSI.  Its characteristical sound can be heard while operating the
mentioned systems.

> [!NOTE]
> I tried different ways to make the sound quieter so it would be less
> annoying, so far without success. Also, I have no reference material
> that would let me reverse engineer the actual numbers, so the
> frequency of the pump actuation is only based on halfway plausible
> physics and the behavior of the Aerosoft models.
>
> I will adjust it as soon as I get my hands on better reference material.

Two nitrogen-precharged accumulators retain pressure so it's available
immediately on demand. The brake accumulator is isolated from the main
system to keep the brakes working even if the main system loses
pressure to a leak.

An emergency hand pump provides a manual backup to re-pressurize the
accumulators if the electric pump fails.

> [!TIP]
> If you need to manually restore hydraulic pressure, you can access the
> emergency hand pump via the Systems tab on the EFB.

If the pressure falls below 800-1000 PSI, expect reduced performance
of the hydraulically powered systems. Flaps will extend more slowly,
brakes will be less effective, and at some point in the future, the
nose-wheel steering will be affected as well.

The mod's hydraulic system has been rebuilt using the sim's modern
("EX1") architecture, and updated to version 2 with the release of
SU6.

> [!CAUTION]

	I don't know how to prime the hydraulics system correctly at the
	beginning of a flight. Over the course of this project, I spent
	several days on this, but I still can't figure out how to keep the
	hydraulics system from draining itself during the cinematics.
