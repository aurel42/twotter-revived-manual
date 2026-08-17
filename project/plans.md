---
title: Plans
review: approved
---

# Plans

## Ongoing (started, but not yet completed)

- System modernization: as Asobo improves sim systems, I adopt the new
  systems to unlock more authentic behavior and simplify custom code

- Custom engine simulation: constant refinements as reference material
  becomes available

- Wear/tear/damage: the scaffolding is present, individual systems
  have been wired up as proof of concept (glareshield annunciators can
  burn out when they're past their lifetime, pitot probe heaters can
  fail when they are powered for an extended time without cooling)

- Implement the full feature set of the TwotterX

- Instrument behavior: some instruments are missing correct power-up
  behavior or real-world idiosyncrasies, they're updated and refined
  as I discover new reference material

- Hydraulics: more faithful behavior as I get new reference material;
  todo: low-pressure behavior of nosewheel steering

## System replacements

- Icing: I tried to make all the de-icing equipment useful, but the
  whole system funnels into a single simvar. To simulate the authentic
  behavior of the de-icing and anti-ice equipment, the icing model of
  the sim has to be replaced with custom code.

- Pitot-static system: the stock sim system is limited and probably
  needs to be replaced with a custom implementation.

## Other plans

- Some form of "Onboarding" in the EFB, like let the user pick the EFB
  mode and a config profile

- Morse idents for all station types that have them, with range and
  noise based on the custom radio propagation model. That should be
  easy with Wwise, right?

## Needs research, references, resources

- Everything propellers: better beta, better sound, working
  autofeather, better behavior of four-blade props

- DHC-6-100 performance, currently guesstimated purely based on SHP

- Needle vibrations

- Wind flutter of control surfaces in walkaround

- Opening doors in flight

- Yaw damper: real-world behavior vs stock sim implementation

- Post-shutdown ticking of cooling exhaust/engine

- Brake heat

- Why does crosswind affect the efficiency of the brakes?

- Improve handling of amphibian and float variants in water

- NiCd battery: thermal runaway

## Unexplored concepts

- GPS/GNS spoofing and jamming
