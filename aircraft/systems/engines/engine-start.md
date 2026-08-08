---
title: Engine Start
review: draft
---

# Engine Start

## From the README

The goal is: pilot can make engines 'splode.
State: laying the groundwork.

Part of the mod is a turbine "simulation", aka "TurbSim".

Let me be perfectly clear about this: while my goal is to present something that is closer to realism than the oversimplified turbine simulation MSFS provides, I don't know at all what I'm doing here. I vibecoded a basic PT6A simulation based on Wikipedia and publicly available research papers (which are already simplified, not based on actual 1st party data, and cover other PT6A variants), without any insights into the P&W's "secret sauce".

I have a bunch of formulas that try to approximate certain parts of the engine, and I feed them with values that I guessed based on what I want the output to look like to hit targets I found in reference videos. I have never seen a hot start, but when the formulas see fuel pooled in the engine before combustion starts, they can make the ITT go red. It's half physics, half fudge.

Known issues: No hung starts, because of sim limitations. No performance degradation or engine failures implemented yet.

- Calibrated engine fuel flow and N2 mapping for accurate power response.
- Adjusted engine torque-to-N1 mapping to align with POH performance data.
- [CHANGED] Added intake deflectors aka inertial separators, enabled doll's eyes indicators, fixed performance penalty for deflectors and ITT penalty for inlet heat. Note: it takes about 80% Ng (POH-faithful) to create the bleed air pressure needed to extend the deflectors.
- Physics-based "simulation" of the PT6A gas path (TurbSim): two-spool dynamics, staged fuel nozzle with characteristic light-off ITT spike, fuel pool that drives hot starts, ignition-delay light-off, T4→T5 gas path with thermocouple lag, block thermal soak-back.
- Per-airframe deterministic jitter spreads engine bias across saved airframes. No two engines are equal.
- Inertial separator needs bleed-air pressure to extend; electrical power is still enough to retract.
- Mitigated the push-from-idle ITT spike by tightening the low-Ng Wf acceleration ceiling.
- A small low-Ng FCU metering-valve servo lag (first-order, ~0.2 s) models the hydromechanical metering valve. No effect on startup (handled by a separate FCU schedule) or cruise/takeoff power.
- Cross-engine-start interlock: starting one engine no longer lets the other engine's running generator back-feed the starter. The opposite generator briefly drops offline during the crank (its GEN annunciator lights) and re-engages once the starter releases. This is a workaround for a sim electrical-system limitation; per the POH (and physics), a cross-engine start isn't possible: the running engine's generator hasn't got the oomph to drive the opposite starter.
- [CHANGED] TurbSim's fuel control unit (FCU) now owns fuel flow.
- [NEW] Added compressor map calibrated against POH performance charts.
- [NEW] Added combustor liner thermal model and breakaway stiction governing the rotor at start.
- [CHANGED] Intake-deflector penalty corrected: the old flat torque/ITT penalty is gone; the remaining effect is a small power reduction from the restricted inlet, matching the POH power charts. Deflectors extend only above ~80 % Ng and need DC power.

Starting a PT6A is the one part of flying this aircraft where the simulation can
bite you, and it is the part we have spent the most effort on. This page is not a
procedure — the in-sim checklist owns that, with per-item hints. This is what the
engine is doing while you follow it, and why it reacts the way it does.

## Operation

**Stabilized Ng is a number, not a feeling.** On battery, the first engine settles
somewhere around 16–18 % Ng; the second sits about a percent lower, because the
first start has already pulled the battery down. Neither figure is a limit — the
limit is the 12 % floor below which fuel must not be introduced (POH §4.6.1). On
a GPU you will see it stabilize higher and get there faster. What matters is that
the needle has *stopped climbing*, not that it has reached any particular mark.

**The hot start is the whole reason step order matters.** Introduce fuel before
the compressor is moving enough air and it does not burn — it pools. The igniters
keep trying, and when the mixture finally comes into range the whole puddle lights
at once. ITT goes wherever the fuel says it goes, which is well past the start
limit and, in a bad one, out of the exhaust stub as visible flame. The earlier you
push the condition lever, the more fuel is waiting when that happens.

This is modelled deliberately, and it is the feature the project was started for.
If you want to see it, push the lever early on purpose. It is worth doing once.

**Losing the boost pumps makes an early start slightly *cooler*.** The
minimum-pressurizing valve upstream of the nozzles cracks later without tank boost
pressure — a couple of percent Ng later — so less fuel reaches the combustor
before light-off. It is a small effect and not a technique; it is simply why two
otherwise identical early starts can differ.

**Generators can trip.** *(Config: **Generators can trip**, off by default.)*
Enabled, this models the real generator control unit rather than the sim's
always-succeeds behaviour:

- Leave the GEN switch ON through the start and the trip is **certain**. The GEN
  caution light stays on; the switch must be held to RESET and released to ON.
- Connect at idle and there is a small chance, which grows with engine age.
- Connect slightly above idle and it is more reliable.
- A RESET can itself fail and need another attempt, exactly as on the aircraft.

The practical habit: leave the generators off through the start and bring them on
once the engine is stable. See [Generators and the GCU](../electrical/generators.md)
for what the switch is actually doing.

**You cannot cross-start.** Starting one engine will not let the other engine's
running generator back-feed the starter. During the crank the opposite generator
drops offline — its GEN annunciator lights — and re-engages when the starter
releases. That is a workaround for a sim electrical-system limitation, but the
outcome is POH-correct: a running engine's generator has not got the output to
turn the opposite starter.

**Watch the voltmeter and the lights, not just the gauges.** Battery terminal
voltage sags under the starter load. The voltmeter dips, the cockpit lighting and
the glareshield annunciators dim with it, and both recover as starter current
tapers off with rising Ng. A weak or cold battery cranks visibly slower and
stabilizes lower — which is precisely the condition in which people introduce fuel
too early.

**No two of your engines are the same.** A deterministic per-airframe jitter
spreads a small bias across saved aircraft, so the airframe you always fly has its
own light-off character and keeps it.

**What a hot start costs you** *(Config: **Reliability**)*. At the default **Off**,
nothing at all — cook it as often as you like. At **Reliable** and above, ITT
excursions accumulate against the engine's condition and surface in the EFB
Maintenance tab, with the wear rate scaling 4× at Unreliable and 16× at Not
Airworthy.

> [!NOTE]
> **Cold soak**
>
> Below about −30 °C the POH has you motor the engine for five seconds without
> introducing fuel, rest the battery for a minute, then start normally
> (§4.6.3). We model the battery recovery, so the rest actually buys you
> something — skipping it gives you a slower crank and a lower stabilized Ng.

## Under the hood

A PT6A is a free-turbine engine: there is no mechanical link between the gas
generator spool and the propeller. The starter only has to turn the compressor,
and the prop stays where it is until there is gas to drive it. That is also why an
Ng floor exists at all — below roughly 12 %, the compressor simply is not pushing
enough air to burn fuel as fast as the nozzles deliver it. Everything unpleasant
about a hot start follows from that one fact.

### What we model

The engine is a physics simulation in `CBDHC6/src/engines/TurbineLogic.cpp`,
stepped at 10 Hz. It is the sole source of the Ng and ITT you see; there is no
scripted shaping layer left anywhere in the project.

The gas generator spool is integrated from an actual force balance — starter
torque, turbine power, compressor absorbed power, windage and friction — rather
than played back from a curve. Compressor behaviour comes from a map in corrected
coordinates, which means altitude is *structural*: the density terms cancel at
equilibrium and the Ng-versus-altitude relationship falls out of the mathematics
instead of being patched in with an exponent.

Fuel is metered by a modelled FCU with two regimes. During the start it runs a
staged-nozzle schedule, so you get the characteristic secondary-nozzle step rather
than the flat delivery the sim would otherwise give you. Once running, it is a
speed governor closing on Ng, with a topping governor trimming the ceiling.

Light-off is where the interesting part lives:

1. Fuel entering below the light-off speed cannot burn, so it **accumulates as a
   pool**. Admission itself is gated by a modelled flow-divider crack, which is
   what the boost-pump effect above comes from.
2. An **ignition delay** — a couple of seconds after fuel is introduced — has to
   elapse before combustion can begin.
3. When it lights, the pool **releases vapour** at a rate set by evaporation off
   the liquid surface. Crucially, that is a release rate, not a burn rate.
4. Where the vapour actually burns is decided station by station, by the air
   available at each: primary zone, then dilution, then downstream of the HPT, and
   whatever is left over leaves the engine unburnt. The downstream share is added
   *after* the turbine drop, so it drives indicated ITT without accelerating the
   compressor — which is why a hot start reads hot without also spooling faster.
   The unburnt remainder is the smoke or flame at the exhaust stub, and it drives
   the exhaust effects you can see from outside.

The consequence worth stating plainly: **hot-start severity is an emergent
property, not a multiplier.** A bigger puddle burns hotter, not merely longer,
because the air at each station runs out sooner. That was not true before July
2026 — the old formulation cancelled mass flow out of the temperature rise
entirely, so a mild and a severe start could not be separated by more than about
45 K no matter what you tuned. Getting that right is the difference between a
scripted spike and an engine.

What you read on the gauge is then a modelled *instrument*, not the gas path. The
ITT indicator is an interstage thermocouple with asymmetric lag, a radiation floor
from the hot block, and soak-back after shutdown. The Ng tach is a charge-pump
frequency-to-voltage converter, which is why it steps at low speed and smooths out
above roughly 17 %. Both mean the number arrives slightly late and slightly low, as
it would on the aircraft.

### Where it stops

Being straight about this: the model is grounded in published research on the PT6
family, not in Pratt & Whitney's data. Several constants were fitted to make the
output match reference video and POH performance charts rather than derived from
first principles. It is a good deal more than a lookup table and a good deal less
than a real engine deck.

Not modelled:

- **Hung starts.** A sim limitation rather than a modelling choice.
- **Compressor stall and surge.** There is a compressor map, but no surge line and
  no stall behaviour.
- **FOD and mechanical failure of the gas path.** Engine wear is tracked, but a
  blade does not leave.
- **Bleed and accessory-gearbox loads** are present as stubs, not as a real
  accounting of shaft power.

If you find behaviour that contradicts the POH, that is a bug and worth reporting
— those are the anchors the whole model is calibrated against.

## From the release notes

Verbatim from `docs/release-notes-*.md`, grouped by the section they appeared in. The tag is the version each item first appeared in.

### General

- Made initialization detection more robust (fixes: TurbSim frequently failing on runway start). *(v2024.2.134)*

### Engines

- DHC-6-100 series variants moved closer to PT6A-20 specs: ITT peak ~1044 °C (vs 1100 °C on the -27), EGT peak and rated fuel flow scaled to the 550 SHP rating. *(v2024.2.142)*
- TurbSim now also follows the sim's "engine is lit" flag, so failure injection, in-flight flameouts and low-bus igniter cutoffs actually stop the alternate ITT/%Ng simulation from continuing to model a running engine. *(v2024.2.142)*
- Added engine exhaust flame and smoke visual effect placeholders. WIP, not hooked up yet. *(v2024.2.142)*
- Calibrated engine fuel flow and N2 mapping for accurate power response. *(v2024.2.x)*
- Adjusted engine torque-to-N1 mapping to align with POH performance data. *(v2024.2.x)*
- Added intake deflectors aka inertial separators, enabled doll's eyes indicators, added torque and ITT penalties for deflectors and inlet heat. Note: it takes about 70% Ng to create the bleed air pressure needed to extend the deflectors. *(v2024.2.x)*
- [NEW] Optional physics-based "simulation" of the PT6A gas path (TurbSim): two-spool dynamics, staged fuel nozzle with characteristic light-off ITT spike, fuel pool that drives hot starts, ignition-delay light-off, T4→T5 gas path with thermocouple lag, block thermal soak-back. Selectable in the EFB Config tab. *(v2024.2.x)*
- [NEW] Per-airframe deterministic jitter spreads engine bias across saved airframes. No two engines are equal. *(v2024.2.x)*
- [CHANGED] Inertial separator needs bleed-air pressure to extend; electrical power is still enough to retract. *(v2024.2.x)*
- Added a compressor map and a fuel control unit (FCU) to TurbSim. The engine now matches the POH power charts across the altitude envelope. *(v2024.3.174)*
- Improved starter and spool-up behavior in some conditions (extreme temperatures, high elevation fields). *(v2024.3.174)*
- Fixed engines flaming out when fuel flow was interrupted momentarily by controller issues or a mid-flight restart by the sim's crash recovery. *(v2024.3.174)*
- Overhauled the generator tripping behavior, making the RESET position of the generator switch matter. *(v2024.3.174)*
- Closed another physics gap in the engine model for more plausible hot starts. *(v2024.3.183)*
- Wired up exhaust smoke and flame effects (placeholder art). *(v2024.3.183)*
- Overhauled the GCU based on real-world feedback: the generator connects at the starter-generator handover (about 46 % Ng) and essentially always comes on line, with a very slim chance of tripping that rises with engine age. A tripped generator still needs a RESET. *(v2024.3.183)*
- Fixed a generator with collapsing output staying connected to the bus and dragging bus voltage down, which could reboot the avionics. *(v2024.3.183)*
- **Mitigated push-from-idle ITT spike.** The Wf acceleration ceiling at low Ng was too loose, allowing quick throttle movements from idle to drive ITT into the red. *(v2024.3.19)*
- Added missing smoothing to the ITT needles. *(v2024.3.19)*
- Fixed issues that caused both engines to quit during the cinematics on a runway start and, occasionally, one engine to quit after the cinematics completed. *(v2024.3.19)*
- **Cross-engine-start interlock.** Starting one engine no longer lets the other engine's running generator back-feed the starter. The opposite generator briefly drops offline during the crank (its GEN annunciator lights) and re-engages once the starter releases. This is a workaround for a limitation of the sim's electrical system. According to the POH (and, I think, physics), cross-engine starts are not possible, because the generator of the running engine does not have enough oomph (that's the technical term) to drive the opposite starter. Consider this a placeholder until I figure out how to simulate this properly. *(v2024.3.68)*
- Closed a physics gap in TurbSim: enthalpic work failed to cool down the gas in some situations. This was the underlying cause of the ITT spikes when the throttle was moved too quickly close to idle. Added the missing term and removed the fudge introduced as workaround (an artificially high FCU valve delay). Normal throttle movements should be responsive again without sending the needle into the red. *(v2024.3.68)*
- TurbSim is now enforced. *(v2024.3.99)*
- Ground tailwind can lead to higher ITT. *(v2024.3.99)*
- Fixed transient ITT bump on firing of the secondary nozzles. *(v2024.3.99)*
- Fixed engine starts on GPU reading the battery voltage for the starter. *(v2024.3.99)*
- Enforced coupling between prop RPM and torque (based on pilot feedback). *(v2024.3.99)*

### What's new since v2024.1.8

- Added an optional physics-based "simulation" of the PT6A gas generator and ITT (TurbSim). *(v2024.2.x)*

### Known issues

- Alternate ITT/%Ng calculation (aka TurbSim) tends to be sensitive to fuel flow delta, especially at low power settings. It helps to move the throttle levers slowly. I have no way of knowing whether this is realistic or exaggerated. Without reference data/video, I don't know what to tune for, and I don't want to make something up, so I'm giving TurbSim some artistic license. *(v2024.2.x)*

### Engines (TurbSim)

- Reanchored to reference data. *(v2024.3.124)*
- Restored intended behavior at altitude (removed a spurious fuel-air-ratio cap). *(v2024.3.124)*
- Fixed slow starter cranking, and cranking when the electrical bus is de-energized. *(v2024.3.124)*

### Propellers

- **New propeller blur.** CCM's prop-blur textures are included in the archive as a separate mod. <3 CCM <3 *(v2024.3.68)*

### Housekeeping

- TurbSim is no longer optional. *(v2024.3.99)*

## L:vars

Variables worth binding to hardware, driving from FSUIPC / Axis&Ohs / SPAD.neXt,
or reading on an external display. **R** is read-only state; **R/W** can be set.
This is a selection, not the full list — the aircraft defines around a thousand
`DHC6_*` names, most of them internal.

| L:var | R/W | Meaning |
|---|---|---|
| `DHC6_ITT_DISPLAY_1` | R | Left ITT as the gauge reads it — thermocouple lag included |
| `DHC6_ITT_DISPLAY_2` | R | Right ITT |
| `DHC6_NG_DISPLAY_1` | R | Left Ng as the sync tach reads it |
| `DHC6_NG_DISPLAY_2` | R | Right Ng |
| `DHC6_ENGINE_BIAS_1` | R | Per-airframe deterministic bias for engine 1 — why no two engines are identical |
| `DHC6_ENGINE_BIAS_2` | R | Same, engine 2 |
| `DHC6_IGNITION_MODE` | R/W | Ignition selector position |
| `DHC6_AF_TEST_ARM` | R/W | Autofeather test arm |
| `DHC6_AUTOFEATHER_TEST` | R/W | Autofeather test |
| `DHC6_FX_SMOKE_LIGHT_1` | R | Exhaust smoke gate, light tier — non-zero during a hot start |
| `DHC6_FX_FLAME_INTENSITY_1` | R | Visible flame at the exhaust stub; only a severe start reaches this |
