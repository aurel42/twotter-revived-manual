---
title: Installation
review: approved
---

# Installation

Because of [previous drama](../project/history.md), this mod is
distributed as an archive containing xdelta files and an install
script. You will need an original, unmodded installation of the
_Aerosoft Twin Otter 1.1.1_ (installed using _Aerosoft One,_ the
legacy Aerosoft installer, or by other means) so the installation
process can **read** the original files and **write** the changed
files to the mod folder. **Your original files will not be modified.**

> [!WARNING]
> **Mod is distributed inert**
>
> Until the install script has been executed, the mod will be inactive.

You will still need to have the base model — the `aerosoft-aircraft-twin-otter`
folder — in your `Community/` folder for the mod to work.

## Contents of the distributed archive

- `charliebravo-aircraftmod-twin-otter` — this is all you need. You must
  unpack this to use the mod; the rest is optional.
- `charliebravo-soundmod-twin-otter-turbineduke` — an optional sound mod.
  Requires that you own the Black Square Turbine Duke.
- `z_aerosoft-twin-otter_propblur_mod` — an optional mod that makes props
  blurry when you're close.
- `z_aerosoft-twin-otter_flat_cameras` — an optional mod that levels out the
  cockpit camera views (the close pilot view and the three captain's instrument
  views), instead of having them tilted down at the panel.
- **`historic_ndb_userpoints.csv`** — can be imported in Little Navmap to show
  historic (decommissioned) NDB beacons.

## Step by step

1. **Unpack** directly into `Community/`, or wherever you keep your Community
   add-ons.
2. **Open** the folder `charliebravo-aircraftmod-twin-otter`.
3. *Optionally, if you're paranoid or they are actually after you:* **examine**
   `install.bat` and `install.ps1`, and overwrite the xdelta executable with a
   version you trust.
4. **Double-click** `install.bat`.
5. **Follow the prompts** and select an unmodified `aerosoft-aircraft-twin-otter`
   folder.

In a perfect world, the changes are applied and you end up with a working mod.

## The sound mod

If you unpacked the soundmod:

1. **Open** the folder `charliebravo-soundmod-twin-otter`.
2. **Double-click** `install.bat`.
3. **Follow the prompts** and select your `bksq-aircraft-turbineduke` folder.

## Liveries

Legacy liveries for the Aerosoft model might work, if the loading
order is carefully managed. Pink textures are supposedly fixed by
using a layout generator tool to regenerate layout.json for MSFS2024.

## If it goes wrong

Before you report an installation error, please try reinstalling a
fresh copy of the Aerosoft Twin Otter and trying with that folder. If
the error persists, please get in touch so we can figure out what's
happening happen. The installer should work with all popular versions
of v1.1.1.
