---
title: Persistence
review: approved
---

# Cockpit and aircraft persistence

The persistence system uses a different lifecycles and layers:

- Config settings persist unconditionally.

- Some settings are only stored after the user opts-in, and they need
  to opt-in for every single session. You can go flying at any time
  without worrying about the aircraft state. Only if you restore the
  state (or, if it's the first time, "Enable persistence") when the
  session begins, these vars are stored for when you opt-in the next
  time.

- Some settings are stored as LocalVars in state.cfg, using a stock
  sim mechanism.

- Some WASM systems store their states separately in files in the
  sim's `WASM/.../work` folder.

- JS/TS-based systems can store data in the cloud, the most prominent
  example are the GNS units.

## Usage

When you use the system for the first time: while on the ground and,
preferably, cold & dark, use the "Enable Persistence" button on the
splashscreen of the EFB (or, if you missed it, the button on the Debug
tab).

In future sessions with this aircraft variant, the button on the EFB
splashscreen will say "Restore State" instead. Click it to restore the
state from the previous session in this variant, or click anywhere
else to skip the restore for an ad-hoc flight without persistence.

While the state is being restored, the EFB will show a message. To
avoid frustration, stand by for a few seconds until the process is
complete. If you change settings while the restore is in progress,
these settings might revert to their stored state.

Resetting the saved state is not recommended and should not be
necessary unless you encounter a bug. 

> [!CAUTION]
> It is possible to reset the state of a variant by locating `state.cfg`
> plus the files in `WASM/.../work` for the variant in your MSFS folder
> and deleting or renaming the files. Only do this if you're comfortable

	with it.

## Known issues

- Some systems don't persist (battery capacity, hydraulic system
  state). I have not figured out how to do this yet. Advice is
  welcome!

> [!CAUTION]
> **Please report bugs!**
>
> If any other systems, switches, knobs don't correctly restore
> their state from a previous session, that's a bug. Please be kind
> and report it.

- "Restore state" does not prevent you from restoring the state with
  running engines, but you probably shouldn't.
