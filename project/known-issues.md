---
title: Known issues
review: approved
---

# Known issues

- Default audio is shit. Aerosoft's Wwise sound bank limits what I can do.

    - Example #1: engine startup sounds are inconsistent and don't
      adapt to the physics; the sound bank fades between effects based
      only on prop RPM, it doesn't care about turbine RPM at
      all. That's why an engine start with feathered props sounds so
      horrible. Also, the interior and exterior sounds are wildly
      different (just compare the interior starter sounds with the
      exterior version) and switching between interior and exterior
      can "restart" audio effects. [The sound mod now addresses all of
      these issues.]
	
    - Example #2: Stock flap sounds are not continuous.
	
- There are four "phantom tanks" visible in the Weight & Balance
  screen of the EFB. I added them to maintain compatibility with
  FSEconomy after enabling the modular fuel system. This is a purely
  cosmetical flaw.

- On the world map, the aircraft thumbnail is missing and a default
  MSFS2024 thumbnail is showing instead. To fix this, I would have to
  create and maintain a separate set of "modern" (MSFS2024-style)
  thumbnails for each variant. That doesn't seem worth it. On most
  other menu screens (e.g. when selecting a variant), the sim should
  use the existing "legacy" thumbnails.

- Generator load sharing: only one starter-generator actually supplies
  the bus at a time. As far as I can tell, MSFS 2024's V2 electrical
  system has no mechanism for two paralleled DC generators to share
  load. The cockpit loadmeter therefore shows Gen 1 near full load and
  Gen 2 near zero in normal operation; that's expected, not a bug. If
  anyone knows how to get real V2 load sharing working, I'd love to
  hear about it.

- ADF: the system can't react to lightning. The sim doesn't expose the
  necessary data.

- FLC mode works when the pitot tube is frozen. Somehow, it knows. Sim
  quirk. (The groundwork to fix this has been done.)

- Pilot and Copilot avatars are not visible, not sure why. Could be a
  sim limitation caused by the combination of a 2020 aircraft with
  2024 systems, or it could be my mistake or lack of understanding.

- KA 51 Slaving Accessory is STILL missing.

- The hydraulics drain during the cinematics and I don't know why or
  how to prevent it. There are workarounds in place to mitigate the
  effects, but I wish they weren't necessary.
