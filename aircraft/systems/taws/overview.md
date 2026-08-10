---
title: Overview
review: approved
---

# Terrain Awareness and Warning System (TAWS)

## Overview

The TAWS system in the mod is inspired by a Honeywell EGPWS suite,
inherited from the base model. It's a custom implementation based
primarily on FAA advisories and the pilot's guide.

Details about the supported modes are [here](regulations.md).

## Configuration

You can select between different TAWS loadouts:

- **Full:** every callout on every variant

- **Typical:** passenger variants get the full set; cargo/utility
  variants get the reduced Class B set

- **Legal minimum:** passenger variants only; cargo/utility variants
  get nothing

- **Class B:** the reduced Class B set everywhere (excessive descent
  rate, altitude loss after takeoff, the "500 feet" callout, and
  MINIMUMS — no terrain, glideslope or bank-angle callouts)

- **Off:** all callouts muted

You can also select between different voice banks based on personal
preference. Only the "Custom" set contains all possible callouts.

## Features

- Declutter governor: an advisory is only repeated if the situation degrades.

- The GPWS INHIB button suppresses some callouts and desensitizes others.

- A TAWS tab in the EFB app (visible in Instructional mode) is trying
  to give you the tools to understand why and when TAWS callouts
  trigger.

- TAWS uses the Twotter's custom navaid system and runway information
  obtained from the sim.
  
> [!TIP]
> I filter out small grass and dirt strips, because they wouldn't

	be covered by the database of an EGPWS suite. To minimize the 
	number of spurious callouts when approaching a small strip, you
	should make use of GPWS INHIB.

## Known issues

- Windsheer alerts are missing (no sim data).

- Lower priority callouts cannot be interrupted by higher priority
  callouts.
