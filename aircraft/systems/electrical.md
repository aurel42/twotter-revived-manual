---
title: Electrics
review: approved
---

# Electrical system

## Overview

Most Twotter systems run on 28V DC, supplied by the main battery, a
GPU cart, or the starter-generators in the engines. AC power for the
remaining systems is supplied by one of two inverters.

> [!TIP]

	The Inverter switch is above and behind the captain, not covered by
	any of the camera views.

## Components

The electrical system is modelled in quite some detail using the
modern system Asobo introduced. It has been upgraded to v2.3 for SU6.
Custom code is used to augment it where necessary.

### Buses

One might call the left DC bus the "main" bus. It's powered by the
battery, the GPU, the left engine's starter-generator and/or (if Bus
Tie is in the NORM position) the right engine's starter-generator, and
it's the bus the voltmeter queries.

Opening the Bus Tie separates the left and right side DC buses, the
right side will go dark unless the right engine is running and its
generator is producing power.

The battery bus is hot (always on). The visible effect: the entrance
lights stay powered with the DC Master switch in the Off position.

The starters draw directly from the battery bus because they need a
ridiculous amount of power.

I added a separate avionics bus which isn't documented in the POH, but
the whole avionics stack is out of scope of the POH, and it seems
plausible that it would have been added as part of the avionics
installation. The separate avionics bus exists mostly to support the
optional avionics master switch (I repurposed the Flight Data Recorder
switch for this).

### Batteries

The default main battery is a 40 Ah NiCd pack. You can swap it for a
36 Ah SLA (sealed lead-acid) battery, which is heavier but doesn't sag
as much while the starters are cranking.

> [!NOTE]

	A magical device keeps the pack topped up unless you select the
	**Realistic battery capacity** option.

Battery temperature is modeled, thermal runaway isn't (yet). The NiCd
heats and cools roughly twice as fast as the lead-acid pack.

The TEST button on the battery temperature gauge drives the needle to
the top of the scale and lights the 150 °F lamp.

To improve the reliability during engine starts in cold weather, a
small auxiliary battery provides power exclusively to engine control
relays and ignition. I think the idea is to keep the engine alive even
if the main battery sags too deep. In the mod, the auxiliary battery
powers the igniters during engine starts and in manual mode.

The emergency lights are powered by an independent battery pack that
is trickle-charged while the bus is running (the trickle charging is
not implemented, the battery is just magically charged like all other
batteries when a flight starts). The emergency lighs are guaranteed to
stay up for at least one hour.

> [!NOTE]

	The batteries as Asobo models them are quite stiff and behave in an
	idealized way. Some custom code helps them to behave, erm, less
	idealized.

### The starter-generators

A starter-generator acts either as starter or as generator, never as
both at the same time. While cranking, it is a motor drawing several
hundred amps from the battery bus.  When the engine can sustain
itself, the field is re-excited and device starts feeding power back.

You cannot cross-start one engine using just the generator of the
other engine. In the real world, physics make this impossible. In the
mod, I enforce this by taking the opposite side generator offline
while the starter is cranking.

### The GCU

The Generator Control Unit (GCU) connects a generator to its DC bus
when it provides power and the Generator switch is in the ON position.

If the **Generators can trip** option is enabled, generators have a
small chance to trip when they come on line. When this happens, the
Generator annunciator will stay lit until the pilot puts the
respective Generator switch to the RESET position for a moment.

> [!NOTE]

	If you take a long time to start up, possibly with heavy electrical
	consumers running, you should consider starting only one engine, and
	then take a few minutes to recharge the battery from that
	generator. You should not attempt an engine start if the battery
	voltage is below 20V before you engage the starter.
	
	Or you can call the GPU cart.

### GPU cart

Setting the DC SOURCE switch to EXT or clicking the GPU button on the
Systems tab of the EFB app summons a GPU cart. It's despawned
automatically when you get ready to move.

> [!NOTE]
> Many thanks to Bagolu for fitting the model to the Twotter!

### Inverters and the AC buses

A switch behind and above the Captain's right shoulder selects either
of two 400-Hz inverters providing 115V and 26V AC for the instruments
that need them. The usual procedure is to switch to the other inverter
before the first flight of the day to balance the lifetime load.

AC-powered devices:

- both attitude indicators
- the Captain's slaved HSI gyro
- the First Officer's directional gyro
- the fuel quantity indicators
- torque, oil pressure and fuel flow gauges

### The gauges

The voltmeter displays the voltage of the left DC bus.

The loadmeter changes scale based on the selected source. In the
center position, it shows the battery load on a 100A scale. When a
generator is selected, the scale changes to 200A. Since the starter
draw exceeds the scale, the loadmeter is disconnected while the
starters are running.

## Known Issues

- I could not figure out how to let two power sources share the load
  on one bus. The solver always prefers one generator and puts the
  complete load on it, while the other generator idles. If you have
  advice, please *share* it (pun intended).

- All batteries are fully charged at the beginning of a flight.  There
  doesn't seem to be a good way to persist their charged capacity.

- Since the circuit breakers are not animated in the visual model, I
  didn't bother to model them.  I might, if it makes sense at some
  point in the future.

## L:vars

Variables worth binding to hardware, driving from FSUIPC / Axis&Ohs / SPAD.neXt,
or reading on an external display. **R** is read-only state; **R/W** can be set.
This is a selection, not the full list — the aircraft defines around a thousand
`DHC6_*` names, most of them internal.

| L:var | R/W | Meaning |
|---|---|---|
| `DHC6_GEN_STATE_1` | R | Generator 1 state — off line, on line, tripped |
| `DHC6_GEN_STATE_2` | R | Generator 2 state |
| `DHC6_GEN_RESET_REQUEST_1` | R/W | Request a GCU reset on generator 1, equivalent to holding the switch to RESET |
| `DHC6_GEN_RESET_REQUEST_2` | R/W | Same, generator 2 |
| `DHC6_BATT_TERM_V` | R | Battery terminal voltage, sag included — dips during the crank |
| `DHC6_BATT_TEMP` | R | Battery temperature |
