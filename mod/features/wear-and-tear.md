---
title: Wear and tear
review: approved
---

# Wear, tear, damage

## Overview

Work on an optional wear/tear/damage system has started. I wired up
two sample systems as a vertical slice, one of them has an actual
effect, albeit a mostly cosmetical one.

There are two ways that components can decay and break:

- Abuse leads to component stress, when stress grows above a
  threshold, it leads to damage. Component stress decays and is
  ephemeral, damage persists and leads to failures. Damaged components
  can be repaired or replaced.
  
- Components age based on time in service or service cycles. They can
  fail when they get close to or exceed their normal lifespan.

The proof-of-concept components:

- Abuse: **Pitot Probe heating elements** can burn out if left on for
  an extended time without airflow in warm ambient conditions.

- Age: **Glareshield Annunciator bulbs** can burn out. The age affects
  the brightness.

- Engines track damage to the hot section. Work in progress.

These systems cover most of the planned wear/tear/damage behaviors:
random, physics-based failures of non-essential systems (like
annunciator lights), and strictly deterministic stress and damage
caused by pilot action, that can lead to (possibly critical) failure.

At this stage, I'm not planning to force random failures of critical
components on users, simply for gameplay reasons.

Don't expect rapid progress with this feature. Each component added to
the system needs research, and asking pilots to break parts of their
plane to provide reference material appears to be frowned upon.

## Usage

The system is controlled by the **Reliability** slider on the EFB Config tab:

- **Off** disables the system and hides the Maintenance tab.

- **Reliable** is the default behavior, I'm aiming for plausible component lifecycles.

- **Unreliable** lets components age four times faster; both time in
  service and service cycles are affected by the multiplier.

- **Not Airworthy** results in 16× wear and you have a higher chance
  of replacement components being refurbished instead of new.

A **Maintenance tab** in the EFB app is available while on the ground
with engines shut down and lists every component that has accrued
stress or damage. 

- **Re-roll** randomizes the aircraft state (production year, TTAF,
  and all component states); 

- **Request Service** runs maintenance and produces a per-component
  report.

- **Reset** returns everything to a factory-new state.

The state persists per variant in the WASM work folder.
