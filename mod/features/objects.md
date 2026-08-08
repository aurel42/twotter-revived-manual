---
title: 3D Objects
review: approved
---

# 3D Objects

## Context

Without access to the project source (which we obviously don't have),
we can't modify the visual model of the base aircraft. What we can do,
is place arbitrary objects into the game world close to the
aircraft. Since they are placed relative to the world and not relative
to the aircraft (= the objects don't move automatically with the
aircraft), this makes most sense for static ground objects.

Thanks to asset donations by Bagolu, we can show **wheel chocks** and a
**GPU cart** while the aircraft is parked.

## Limitations

We place the objects so they align with the plane (in the geometrical
sense) the aircraft sits on. This means they work best on a flat
apron. If the terrain around the aircraft is irregular, objects might
float in the air or sink into the ground.

> [!NOTE]
> **For devs**
>
> If you're curious how we spawn objects on a sloped surface, have a
> look at the Sidecar WASM system.
