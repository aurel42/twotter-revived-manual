---
title: Overview
review: approved
---

# Autopilot overview

The autopilot is one of the areas where I'm claiming artistic license. While I fixed a couple of "realism gaps" with the default KAP 140, I also created new ones.

Aerosoft put the Bendix/King KAP 140 in the Twotter for two reasons, afaik: users are familiar with it and the mission statement for the 2020 version was to use default avionics wherever possible. The visual model has all the remnants of the Collins AP-106 (I think) simulated in the Aerosoft Twin Otter X Extended: the annunciators, the mode and altitude panels, the multifunction "knob" on the control column pedestal, the YD button.

This means the 2020 Twotter's AP is already an unholy abomination, and I leaned into that.

## History

The autopilot is a prime example of how this project grew as I learned more about the sim. First, I used stick and carrot, controlling the stock autopilot by manipulating the selected altitude and the heading bug while showing the numbers and modes the user expected on the display. That worked well enough, but had some limitations, so I tried taking over the whole autopilot the same way the Working Title framework does, but I missed a single, important switch, causing all my experiments to fail, so I shelved this approach. I went back to stick and carrot and invented more workarounds to allow me to tune the AP behavior further.

It took an embarrassing amount of time for me to notice what *actually* drove the AP in the mod since the very first version.

> [!NOTE]
> **The very first commit message in this project**
>
> [v2024.0.1] - 2026-03-06
>
> - Updated GNS530 to WT530

As I understand it, whenever Working Title avionics are in play, they put the sim AP in *managed mode* and control pitch and bank directly. So I pulled the GNS units into the mod and started customizing the directors, moving from hacks and workarounds to a proper solution. But the amount of changes were significant which, for one, makes it hard to follow Working Title's updates closely, and I'm not really comfortable working with JS/TS. So the current goal is to have the state machines that run the AP in WASM, where I can test the  thoroughly without even starting the sim, and to reduce the number of patches I apply to the Working Title code. What remains is just a thin layer around the parts I want to replace, that says "talk to WASM instead", and the actual changes to the GNS user interface.

> [!NOTE]
> **Don't let me confuse you**
>
> Whether I use "selected altitude" or "altitude preselect", I mean
> the same thing: the "target altitude" selected by the pilot or set
> when entering alt mode.

## Collins AP features

- IAS/FLC mode, yaw damper, and flight director.
- The flight director command bars on the attitude indicator can be hidden via the **Disable Flight Director** toggle in the EFB Config tab.
- Mode buttons and altitude selector are synced with the KAP 140.
- The Collins annunciators are somehow magically driven by the KAP 140.
- The pitch-trim arrow indicator is now a runaway-trim warning indicator (flashes for end-of-travel and same-direction-≥2 s commands). Small trim corrections should no longer cause epileptic seizures in photosensitive pilots.
- Enabled autopilot annunciators for approach arm/capture when using GPS navigation.

## Working Title framework features

The GNS units gained two features which are completely unrealistic (they are present in the G1000NXi, but not in the much older GNS units). You can turn them off if you prefer more authentic behavior.

- Coupled VNAV: whenever you select an approach with vertical guidance, VNAV is armed.
    - You can disable VNAV by pressing the VNAV button on the GNS 530, enabling cursor mode, selecting "DISARM VNAV" and confirming with ENT.
    - VNAV respects the selected altitude. VNAV will not initiate a descent if you are in ALT HOLD at the selected altitude. If VNAV is armed but cannot descend because of this restriction, the KAP 140 display will alert you by slow-blinking the VNV-ARM flag. Reduce the selected altitude to the next restraint or the expected altitude for capturing a GS or GP, and verify that VNV-ARM is no longer blinking and that the VNAV page on the GNS 530 displays a plausible time to TOD.
    - VNAV will not engage if you're too far from course.
    - While armed, the VNAV page on the GNS 530 should display a time to the next top of descent (TOD).
    - While active, the VNAV page on the GNS 530 should display the next altitude restraint VNAV is descending to.
    - Engage APR mode while descending toward the GS or GP for a smooth capture.
- VISUAL approaches: just like in the G1000NXi, you can set up a "VISUAL" approach for any runway of any airport or airfield.
    - VISUAL approaches have vertical guidance, so VNAV is armed for them. Select an altitude of 800-1000 ft above field elevation for a smooth capture of the glide path (GP).

> [!WARNING]
> **VISUAL approaches are guidance-only**
>
> When you are using VISUAL approaches, you alone are responsible to
> ensure terrain clearance and object avoidance. In difficult
> terrain, relying on a VISUAL appraoch can and probably will kill
> you.
