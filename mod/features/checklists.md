---
title: Checklists
review: approved
---

# Checklists

## Overview

The mod delivers a custom checklist system as part of the EFB
app. Three ways to run a checklist:

- Default: You manage the checklist and tick off every item yourself.

- Interactive: You run the checklist, the co-pilot confirms and ticks
  off the items he confirmed. If the co-pilot gets stuck, you can tick
  the item yourself to progress.

- Autocomplete: The co-pilot runs the checklists, applies all
  settings, runs multi-step tests, confirms every item and ticks
  it. Autocomplete is not available for some in-flight checklists. 

> [!WARNING]
> The co-pilot tries to be smart about "as required" items, but, as
> always, the responsibility lies with the captain. Verify the
> co-pilot's judgment calls are sound, especially regarding de-icing
> equipment.

## Notes

- Some items will never autocomplete, for example the passenger
  briefing.  Tick the item after you completed the briefing to let the
  co-pilot resume.

- A checklist in autocomplete will continue to run even if the EFB is 
  closed or you switch to an outside view. It's fun to let the engine start 
  checklist run on autocomplete while watching the startup in drone view.
  Especially with the sound mod.

- The co-pilot callouts are intentionally annoying for maximum
  authenticity. They can be muted, though, on the Config tab or using
  the Mute button on the Checklists tab.

- Float, amphibian and ski variants get their own POH-supplement
  procedures; cargo variants skip cabin/passenger items.

- Checklists are aware of the avionics master switch and use it if
  it's enabled.

- Take-Off checks: The 85 % Ng pause during start can be skipped.

- When opening the Checklists tab for the first time, it displays
  prominently **"For Simulation Use Only",** just in case you forget
  that you're flying a pixel plane.

## L:vars

`DHC6_CFG_COPILOT_MUTE` — 0 (default, callouts audible), 1 (callouts muted)
