---
title: Generators and the GCU
review: draft
---

# Generators and the GCU

## From the README

The goal was: create the dimming effect that Aerosoft captured, but organically. Instead of scripting the effect based on the starter switch, it should be based on sagging bus voltage.
State: Achieved in the first Milestone release v2024.3.68.

Known issues: I could not figure out how to let two power sources share the load on one bus. The solver always prefers one engine and puts the complete load on it, while the other generator idles. If you have advice, please share it. All batteries are always full at the beginning of a flight (unless you save the state manually in a .flt file, I think.) It also takes a couple of seconds for the first generator to come online after toggling its switch, and during GPU starts the voltmeter can move erratically.

- Rebuilt electrical system using v2.2 for more reasonable battery drain and consumer loads.
- Added opt-in option for realistic battery drain. By default, a magical device keeps the batteries charged.
- Added option to select NiCd or SLA battery with different capacities and behaviors.
- Added invisible GPU cart that can be selected using the DC Source selector on the ground. (Same behavior as before, but with different mechanics to satisfy MSFS2024.)
- Added battery packs to emergency lighting. Armed emergency lights turn on when main power is lost and are powered for at least 15 minutes. (Limitation: the batteries are magically charged by the sim when the flight starts, trickle-charge is currently missing because of... erm... topological challenges.)
- Adjusted power consumption of most components, based on POH values, research, or gut feeling.
- Generator Control Unit "simulation": GCU faults can trip a generator, requiring an explicit RESET to bring it back online; tripping can fire on undervoltage if enabled in the EFB Config tab.
- Battery temperature thermal model: ambient soak, charge/discharge heating, and cooling.
- BUS TIE switch correctly couples or isolates the two main DC buses symmetrically.
- Interior light dimming reacts to bus load.
- Annunciator brightness now uses a per-lamp aging factor.
- Pitot heat split into two independent circuits. Note: the sim still provides only a single pitot-static system feeding both airspeed indicators. That separation is future work.
- Battery terminal voltage now sags under heavy load. It's most visible during engine start — the voltmeter dips and the cockpit lighting (including the glareshield annunciators) dims as the starter pulls the bus down. Starter current tapers as the engine spools up. A weak battery cranks noticeably slower and takes longer to stabilize at a lower %Ng.
- Fixed the loadmeter-needle animation that barely moved under idle load; it now displays POH-correct values (the scale differs depending on whether the bus is on battery or on generator, see POH).
- Moved the engine starters to the battery bus to align with the POH electrical topology, and added a back-EMF starter model.
- The GPU cart is no longer invisible. There's a GPU button on the EFB Systems tab that calls one out to the aircraft, and selecting EXT on the DC Source selector calls one too. It parks off the rear-left fuselage with its cable plugged into the external power receptacle. Switching away from EXT does *not* send it away — press the button for that, which it refuses while EXT is selected and the cart is actually carrying the load. It leaves for good the moment you start rolling. Wheeled variants only. Many thanks to Bagolu for providing the model!

Two starter-generators, one per engine, each feeding its own main DC bus. The
same machine that cranks the engine becomes its generator once the start is over,
which is why this page and [Engine Start](../engines/engine-start.md) keep
referring to each other.

## Operation

**The GEN switch is a request, not a command.** *(Config: **Generators can trip**,
off by default.)* With the option off, generators come on line whenever you ask
and stay there — the sim's behaviour. With it on, a generator control unit sits
between you and the bus, and it can refuse:

- **GEN switch left ON through the start** — the trip is certain. This is the one
  to build a habit around.
- **Connecting at idle** — a small chance of a trip, rising with engine age.
- **Connecting slightly above idle** — the most reliable way to do it.

A tripped generator leaves its GEN caution light on. Hold the switch to RESET,
then release it to ON. The RESET itself can fail and need repeating; that is the
real unit's behaviour, not an artificial retry tax.

Optionally the GCU will also trip on undervoltage, which turns a sagging bus into
a cascade rather than a slow fade.

**The loadmeter reads on two different scales.** Whether the bus is on battery or
on generator changes what full-scale means, so the same needle position is not the
same current in both cases. The POH scale is the one we drive it to.

**BUS TIE couples the two main buses symmetrically.** With it closed, either
generator can carry both sides; with it open they are independent. It behaves the
same in both directions, which was not true before v2024.3.130.

**Starters live on the battery bus.** That is the POH topology and it has a
visible consequence: the crank pulls the battery down directly, so the voltmeter
dips and the cockpit and glareshield lighting dim with it. Starter current tapers
as Ng rises, and we model the back-EMF that causes it, so the sag recovers
progressively rather than snapping back when the switch releases.

There is a GPU cart, and you can see it. Selecting EXT calls one out, and so does
the GPU button on the EFB Systems tab — the button exists because wanting a cart
on stand and wanting to draw power from it are two different things. Once it is
there, switching away from EXT does not send it away; press the button for that.
It refuses while EXT is selected and the cart is carrying the load, which is the
one case where pulling the plug would be a party trick rather than a procedure.
Start rolling and the cart is gone for good — it is parked in the world, not
attached to the aircraft, and the alternative was watching it skate along behind
you. Wheeled variants only: the cable was fitted to that fuselage stance.

> [!WARNING]
> **Known limitation: no load sharing**
>
> Only one starter-generator actually supplies the bus at a time. As far as we
> can tell, the sim's V2 electrical system has no mechanism for two paralleled
> DC generators to share load — the solver always picks one. The cockpit
> loadmeter therefore shows generator 1 near full load and generator 2 near
> zero in normal operation. That is expected, not a fault you have caused. If
> you know how to make V2 share load properly, please get in touch.

## Under the hood

A starter-generator is one machine wired two ways. During the start it is a motor
drawing several hundred amps from the battery bus; once the engine can sustain
itself, the field is re-excited and the same rotor becomes a shunt generator
feeding the bus it was drawing from. A GCU sits on the output and decides whether
that generator is fit to be connected — voltage in band, polarity correct, no
reverse current — and drops it off line if not.

We build this on the sim's V2 modular electrical system, which gives us real
buses, real consumers and a real solver, and then add the parts V2 has no concept
of. The trip model is ours: a probability per connection attempt, conditioned on
engine speed and accumulated engine age, with a separate and independently
fallible RESET path. The battery is a modelled cell stack with terminal voltage
that sags under load and recovers, plus a thermal model that soaks toward ambient
and self-heats on charge and discharge — a cold battery genuinely cranks worse.

Where it stops: load sharing, as above, is a sim constraint we have not found a
way around. Trickle charge into the emergency-lighting packs is missing for
topological reasons — those batteries start each flight full whether they earned
it or not. And the generators are modelled as ideal sources once connected; there
is no field-winding behaviour, no ripple, and no frequency-domain anything.

## From the release notes

Verbatim from `docs/release-notes-*.md`, grouped by the section they appeared in. The tag is the version each item first appeared in.

### Annunciators / Glareshield

- DEAD REC annunciator now also lights on NAV1 cone-of-confusion. *(v2024.2.134)*
- Boost pump annunciators now key on FUELSYSTEM PUMP ACTIVE *(v2024.2.134)*
- Pneumatic Low Press, 400 Cycle, Fwd/Aft Fuel Low, Aux Battery Active, generator-bus annunciators all rewired to authoritative SimVars/L:vars. *(v2024.2.134)*

### What's new since v2024.1.8

- Rebuilt the electrical system on the V2 modular architecture. *(v2024.2.x)*

### Electrical system

- Rebuilt electrical system using v2.2 for more reasonable battery drain and consumer loads. *(v2024.2.x)*
- Added opt-in option for realistic battery drain. By default, a magical device keeps the batteries charged. *(v2024.2.x)*
- Added option to select NiCd or SLA battery with different capacities and behaviors. *(v2024.2.x)*
- Added invisible GPU cart that can be selected using the DC Source selector on the ground. (Same behavior as before, but with different mechanics to satisfy MSFS2024.) *(v2024.2.x)*
- Added battery packs to emergency lighting. Armed emergency lights turn on when main power is lost and are powered for at least 15 minutes. (Limitation: the batteries are magically charged by the sim when the flight starts, trickle-charge is currently missing because of... erm... topological challenges.) *(v2024.2.x)*
- Adjusted power consumption of most components, based on POH values, research, or gut feeling. *(v2024.2.x)*
- [NEW] Generator Control Unit "simulation": GCU faults can trip a generator, requiring an explicit RESET to bring it back online; tripping can fire on undervoltage if enabled in the EFB Config tab. *(v2024.2.x)*
- [NEW] Battery temperature thermal model: ambient soak, charge/discharge heating, and cooling. *(v2024.2.x)*
- [NEW] BUS TIE switch correctly couples or isolates the two main DC buses symmetrically. *(v2024.2.x)*
- [NEW] Interior light dimming reacts to bus load. *(v2024.2.x)*
- [NEW] Annunciator brightness now uses a per-lamp aging factor. *(v2024.2.x)*
- Pitot heat split into two independent circuits. The sim only provides a single pitot-static system that supplies both airspeed indicators. Future work must separate the pitot probes so a single heating element can fail leading to a single airspeed indicator getting stuck. *(v2024.3.19)*
- **Generators no longer source power with the DC Master in OFF.** Fixed cases where generator output reached the bus while the master switch was closed. *(v2024.3.19)*
- Changed GPU breaker from 200A to 400A and fixed GPU being disconnected early at engine idle with both generators switched off. This should prevent avionics from rebooting during GPU-powered engine starts. *(v2024.3.19)*
- **Battery voltage now sags under heavy load.** Most visible during engine start — the voltmeter dips and the cockpit lighting dims as the starter pulls the bus down. Starters draw a realistic current that tapers as the engine spools up. A weak battery cranks noticeably slower and takes longer to stabilize at lower %Ng. *(v2024.3.68)*
- **Overhauled idle load.** The load of a number of components was estimated on the high side and corrected downwards during a research pass. *(v2024.3.68)*
- Fixed an animation error that caused the loadmeter needle to barely move under idle load. It should now display POH-correct values. Note that the scale is different based on whether the bus is on battery or on generator, see POH for details. *(v2024.3.68)*
- As a result of these two fixes, idle load will now **look** much larger, but the battery will nonetheless drain significantly slower. *(v2024.3.68)*
- Known issues: it takes a couple of seconds for the first generator to come online after toggling the switch; during GPU starts, the voltmeter can move erratically *(v2024.3.68)*

### Bug fixes

- Corrected emergency exit lights logic; they now illuminate correctly in the ARM position upon loss of power. *(v2024.2.x)*
- [NEW] Annunciator lights work again (regression introduced by SU5 release 1.7.21.0). *(v2024.2.x)*

### Known issues

- Generator load sharing: only one starter-generator actually supplies the bus at a time. As far as I can tell, MSFS 2024's V2 electrical system has no mechanism for two paralleled DC generators to share load. The cockpit loadmeter therefore shows Gen 1 near full load and Gen 2 near zero in normal operation; that's expected, not a bug. If anyone knows how to get real V2 load sharing working, I'd love to hear about it. *(v2024.2.x)*

## Config options

Set from the EFB **Config** tab. Each option is documented on exactly one page; see [Configuration](../../../mod/configuration.md) for how profiles and the per-variant override work.

### Generators can trip

`DHC6_CFG_GCU_LATCH` — default: **off**

Models the real generator control unit: the generator connects at the starter-generator handover (about 46 % Ng) and essentially always comes on line, with a slim chance of tripping off that rises with engine age. A tripped generator needs a RESET (hold to RESET, release to ON). Off keeps the generators always on line.

### Realistic battery capacity

`DHC6_CFG_REALISTIC_BATTERY` — default: **off**

Gives the battery a real amp-hour budget. Off means unlimited capacity.

### Battery Replacement (NiCad to SLA)

`DHC6_CFG_BATTERY_SLA` — default: **off**

Swaps the standard NiCad pack for a sealed lead-acid battery with different chemistry and higher weight.

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
| `DHC6_AVIONICS_MASTER` | R/W | Avionics rack power, when the FDR-as-avionics-master option is on |
| `DHC6_FDR_STATUS` | R | Flight Data Recorder switch position |
