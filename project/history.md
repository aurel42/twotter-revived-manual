---
title: Project History
review: approved
---

# History

## Pre-History

You know how you have your best flight simming moments that you will (hopefully) never forget and that, after years and years, still fill you with joy when you recall them? One of those moments for me was my first hot start in a Twin Otter in FSX or P3D, about a decade ago. That was the day I learned to treat engines with respect. I started actively looking for aircraft that could break if treated badly, discovered RealAir and A2A, and learned to appreciate deeper simulation.

So the history of this project started, in a way, with the incredible _Aerosoft Twin Otter X Extended_ for FSX/P3D. Aerosoft already had a Twin Otter add-on for FSX (released in 2008), and out of some stroke of genius, they tasked Finn Jacobsen with creating an improved version of it. They gave him carte blanche to define the feature set and the result, released in 2013, showed how passionate he was about the project. It was an amalgamation of existing and new concepts coming together in a complete, well-rounded package. And it was not only supported but actively improved and extended for years.

The Twotter Revived project inherits a design philosophy from the "TwotterX": don't accept sim limitations as blockers, fix them using custom systems.

In 2022, Aerosoft released the _Aerosoft Twin Otter (MSFS)_ to mixed reception. As a TwotterX user, I felt right at home in the cockpit. The KAP 140 seemed like an odd choice for an aircraft of this size, but the visual model overall was excellent. The flight model and systems were intentionally simplified to allow for quicker development and easy accessibility, targeting the mass market created by the release of MSFS on PC and later on Xbox.

The community tried to fix issues with the initial release, but Aerosoft took a heavy-handed stance and had the mod taken down from flightsim.to.

## History has Consequences

Twotter Revived takes extensive steps to avoid distributing any original intellectual property so we don't give the current, as of yet undetermined, rights holder reason to object to the project. Instead of distributing any modified files, we distribute machine-readable instructions on how the files need to be modified ("insert the character X in this position, delete that line").

**What this means for you**

- To use Twotter Revived, you will need to have a pristine, unmodified version of the last official release (v1.1.1) of the base aircraft installed in your Community/ folder, which means that the mod cannot work if you purchased the aircraft on the in-game Marketplace.
- To activate, or more precisely, assemble the mod, you need to run the install script in the mod folder. Point the script at the `aerosoft-aircraft-twin-otter` folder. The files in this folder will not be modified in any way. The install script reads the original files, applies the changes using `xdelta`, a standard tool commonly used for this task in many modding communities, and creates the actual mod on your disk. You cannot redistribute the mod in the *assembled form,* because it contains intellectual property of the rights holder of the base aircraft.

## Actual Project History

- The work on Twotter Revived started in March 2026.
- The initial public release (v2024.0.26) on 20/03/2026 came with Working Title's GNS, hot start "theater", cockpit persistence, an EFB load manager, modular fuel system, and it fixed a few v1.1.1 bugs that had never been addressed. The first five-star review is dated 19/03/2026, probably a timezone thing.
- In April, I broke the original Aerosoft WASM module that was still driving the hydraulics pump, TAWS, and the two clocks. Instead of trying to fix it, I started to replace it. Up to that point, I had maintained a quick release schedule with small iterations, but for the next four weeks, the Twotter was grounded. Your mechanic ripped out the electrical and hydraulic system and rebuilt them from the ground up using the modern, more capable 2024 systems. I started moving systems that had become hard to maintain from classic (hard to read, hard to test) RPN-XML to the WASM module. I built a testing harness so all the C++ code could be checked offline (without having to load the sim), making development not just much quicker, but also more robust.
- In May, v2024.2.113 saw the light of day. It had all those drastic changes, a first attempt at an autopilot overhaul (focused on the KAP 140), extensive work to make instruments feel more authentic, and the first version of the custom engine simulation. 
- The v2024.2.x release cycle ended with "The Dunce Cap Release". The EFB app I had created for Twotter Revived had two bugs that played together to famously "cripple the TriStar". The first bug kept the app active when other aircraft were loaded, the second bug caused the app to change the state of an electrical circuit on some of these aircraft. When I got reports by community members that had bisected problems with the recently released TriStar and found my mod was responsible, I contacted iniBuilds support and worked with them to localize and fix the issue within a day. Lesson learned.
- v2024.3.19 added a vertical slice of the planned Wear/Tear/Damage system.
- v2024.3.47 released with an all new flight model contributed by CanadianCaptainMoustache. It overhauled flight model, flaps, props, steering, and suspension. This was a game changer for the mod. It restored the classic nose-down Twotter stance and added a new level of authenticity and credibility to the mod. ADF/RMI got the first overhaul, leading to more drastic changes down the line.
- v2024.3.68 was the first "milestone release". The milestone was that the glareshield annunciators now dimmed when the starter was engaged. All Twotter add-ons have this effect and implement it as "if the starter is running, then dim". The milestone goal had been to create this behavior organically, using the new electrical system and the effect of a physically plausible power draw of a starter-generator (trying to get a turbine moving) on the battery. For most users, it's a tiny thing. For me, it proved that plausibly reverse-engineered custom systems combine organically to let plausible behavior emerge and that the groundwork paid off.
- In v2024.3.99, we committed to the custom engine simulation and dropped support for the "legacy" flight model. We replaced the stock sim navaids with custom radio code. Other features include a custom checklist system that brought back autocomplete and interactive modes.
- v2024.3.174 brought another game changer: an optional sound mod lets owners of the Black Square Turbine Duke re-use its beautiful soundbank by Boris Audio Works for the Twotter. The audio is wired up to the custom systems and actually reflects the aircraft state.
- Current status: we're in the v2024.3.x cycle, still adding and refining systems.
