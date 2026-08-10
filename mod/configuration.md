---
title: Configurable settings
review: approved
---

# Configurable settings

Everything on this page is set on the Config tab in the 
["Twotter" EFB app](features/efb.md).

The default settings are either sim defaults or they enable convenience features.
The non-default settings strive to be more realistic, and might end up being more annoying.

## Profiles

The profile buttons at the top of the Config tab reset most of your settings to one of these:

- **Classic**: the original Aerosoft experience. Convenience features on, realism features off, full TAWS with the Aerosoft sound bank, stock-faithful GNS.
- **Recommended**: what I fly. Realistic battery, generators, gyros and parking brake, with the GNS and HSI convenience features kept.
- **Realistic**: All realism options are on. Every convenience beyond the real KAP-140, GNS 530W and KI-525A is switched off.

Pressing a button asks whether to apply the profile to this variant only or fleet-wide. If your settings match none of the three, the tab reports the profile as *Custom*.

| Option | Classic | Recommended | Realistic |
|---|---|---|---|
| Disable relaxed NAV arming | off | off | on |
| Enable KAP-140 voice messages | off | on | *untouched* |
| Disable Flight Director | off | on | on |
| Enable historic NDBs | off | on | off |
| Disable visual approaches | on | off | on |
| Disable coupled VNAV | on | off | on |
| TAWS installation | Full | Reduced | Legal minimum |
| TAWS sound bank | Aerosoft | Custom | Custom |
| TAWS volume | *untouched* | -3 dB | -3 dB |
| Realistic battery capacity | off | on | on |
| Realistic parking brake | off | on | on |
| Generators can trip | off | on | on |
| Gyros take time to spin up | off | on | on |
| Disable Auto Fast-Erect | off | on | on |
| Disable OBS Auto-Slew | off | off | on |
| Disable ADF Card Slaving | off | off | on |

Two of the gaps are deliberate. Classic leaves the TAWS volume alone because the
Aerosoft sound bank ignores it. Realistic leaves the KAP-140 voice messages
alone because the real unit's voice is an installation option: a silent unit and
a speaking one are equally authentic, and the realism axis has nothing to say
about it.

Some options are in no profile at all, they're strictly personal
preference: **Use FDR switch as Avionics switch**, **Mute copilot
callouts**, **Battery Replacement (NiCad to SLA)**, and
**Reliability**.

## Options

Some options have an override checkbox on the left: leave it unchecked
and the setting is shared across all variants that don't override it;
check it and that variant keeps its own value, independent of the
rest. This is intended to allow you further individualize the aircraft
variants you fly.

All other options are fleet-wide.

Every switch below is off by default.

### Avionics

| Option | What turning it on does |
|---|---|
| Use FDR switch as Avionics switch | Repurposes the unused Flight Data Recorder switch as an avionics master for the aftermarket stack: GNS 530/430, KAP-140, transponder, ADF and audio panel. Round gauges are unaffected.<br>`DHC6_CFG_FDR_AS_AVIONICS_MASTER` |
| Disable relaxed NAV arming | Removes the KAP-140's NAV pre-arm. NAV mode will be rejected if not in range of the course.<br>`DHC6_CFG_AP_STRICT_NAVARM` |
| Enable KAP-140 voice messages | Adds the unit's three optional spoken messages: "Altitude" 1000 ft before the preselect, "Leaving altitude" 200 ft after departing it, and "Autopilot" on disconnect. They accompany the tones rather than replacing them. The two standard trim messages are always on either way.<br>`DHC6_CFG_KAP140_VOICE` |
| Disable Flight Director | Hides the command bars on the attitude indicator. The flight director itself still engages.<br>`DHC6_CFG_DISABLE_FD` |
| Disable visual approaches | Hides the GNS per-runway VISUAL approaches. The real 530W/430W has none, so this is the stock-faithful setting.<br>`DHC6_CFG_DISABLE_GNS_VISUAL_APPR` |
| Disable coupled VNAV | Takes away the coupled VNAV (PATH) descent, leaving the advisory-only VNAV the real 530W has.<br>`DHC6_CFG_DISABLE_GNS_COUPLED_VNAV` |
| Enable historic NDBs | Adds historic NDB beacons that modern navdata has dropped. These take precedence, and the sim's navdata is used only for beacons the historic set doesn't have.<br>`DHC6_CFG_NDB_SOURCE` |

> [!CAUTION]
> The GNS options only take effect when the GNS starts
> up. Currently, this needs you to start (or restart) a
> flight. A power cycle will most likely not do.

### TAWS

| Option | Positions |
|---|---|
| Installation | **Full** (default) — Class A on every aircraft, the full callout set. **Reduced** — passenger variants get Class A, cargo and utility get Class B. **Legal minimum** — passenger variants get Class A, everything else gets nothing. **Class B** — no terrain, glideslope or bank-angle alerts anywhere. **Off** — all callouts muted.<br>`DHC6_CFG_GPWS` |
| Sound bank | Aerosoft (default), FlyByWire, Custom<br>`DHC6_CFG_GPWS_AUDIOBANK` |
| Volume | You know how a volume slider works.<br>`DHC6_CFG_GPWS_VOLUME` |

Volume applies to the FlyByWire and Custom banks only. The Aerosoft bank ignores
it.

> [!NOTE]
> Letting go of the Volume slider plays a callout so you can hear the level.
> It picks one at random from the full set, which includes "Pull up" and
> "Terrain ahead, pull up". Pro tip: Don't change the volume while airborne.

### Systems

| Option | What turning it on does |
|---|---|
| Realistic battery capacity | Gives the battery a real amp-hour budget that the enabled consumers draw down. Off means unlimited capacity. If you flatten it, change the chemistry or set the source switch to EXT to summon a GPU cart.<br>`DHC6_CFG_REALISTIC_BATTERY` |
| Battery Replacement (NiCad to SLA) | Swaps the standard NiCad pack for a sealed lead-acid one: different chemistry, more capacity, more weight.<br>`DHC6_CFG_BATTERY_SLA` |
| Realistic parking brake | Models the real DHC-6 sequence: fully depress both toe brakes, pull the lever, release the toe brakes. If the latch doesn't catch, the handle snaps back and you try again.<br>`DHC6_CFG_REALISTIC_PARKING_BRAKE` |

The SLA pack weighs 20 lb more, and that weight is added to the
aircraft as soon as you select it.

### Engines

| Option | What turning it on does |
|---|---|
| Generators can trip | Generators have a small chance to trip when they come online. The annunciator will stay illuminated. A tripped generator stays off until you RESET it: hold the GEN switch to RESET, release to ON.<br>`DHC6_CFG_GCU_LATCH` |

### Instruments

| Option | What turning it on does |
|---|---|
| Gyros take time to spin up | Gyros spool up from rest instead of coming up aligned, which takes a couple of minutes from cold. Pull to Erect shortens it for the attitude indicator, and the HSI can be synced to the whiskey compass from the EFB Systems tab.<br>`DHC6_CFG_REAL_GYROS` |
| Disable Auto Fast-Erect | Stops the attitude indicator from pulling its own fast-erect knob during cold-and-dark erection. Pull it yourself or wait a long time for the horizon to settle.<br>`DHC6_CFG_DISABLE_AUTO_FAST_ERECT` |
| Disable OBS Auto-Slew | The HSI course pointer stops rotating itself to the active GPS leg. Set the course yourself.<br>`DHC6_CFG_DISABLE_OBS_AUTO_SLEW` |
| Disable ADF Card Slaving | The ADF compass card stops following the HSI heading, turning a KI-227-01 into a KI-227-00 you set with the HDG knob.<br>`DHC6_CFG_DISABLE_ADF_CARD_SLAVE` |

Disable Auto Fast-Erect only means anything with realistic spin-up on, so the
tab greys it out when Gyros take time to spin up is off. It keeps its stored
value while greyed.

### Checklists

| Option | What turning it on does |
|---|---|
| Mute copilot callouts | Silences the copilot's spoken checklist responses, including "Checklist complete". The checklist still runs at the same pace.<br>`DHC6_CFG_COPILOT_MUTE` |

This is the same setting as the **Mute** button in the Checklist tab. Either one
moves the other.

### Wear & Tear

| Option | Positions |
|---|---|
| Reliability | **Off** (default) — the wear and tear system is disabled and the Maintenance tab is hidden. **Reliable** — nominal wear rate, experienced mechanic, risk-averse management. **Unreliable** — 4× wear. **Not Airworthy** — 16× wear, an inexperienced mechanic and poor access to new parts.<br>`DHC6_CFG_RELIABILITY` |

Reliability is stored per airframe out of the box, so each variant ages on its
own unless you uncheck its override.

> [!NOTE]
> Wear & Tear & Damage is work in progess, and only few systems have been hooked up to it.
