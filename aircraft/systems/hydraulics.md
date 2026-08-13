---
title: Hydraulic system
review: approved
---

# Hydraulic system

The hydraulic system powers the flaps, nose-wheel steering, and toe
brakes from a single electrical pump that cycles on and off via a
pressure switch to keep the system at 1550 ± 50 PSI.  Its
characteristical sound can be heard while operating the mentioned
systems.

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

The mod's hydraulic system has been rebuilt using the sim's modern
("EX1") architecture.

> [!NOTE]
> I haven't figured out how to prime the hydraulics when the aircraft
> loads. Every flight technically starts with an empty system. To get
> around this sim limitation (or dev limitation, I can't be sure), an
> invisible mechanic uses the hand pump to prime the system for you on
> load.
>
> If you need to manually restore hydraulic pressure, you can access the
> emergency hand pump via the Systems tab on the EFB.
