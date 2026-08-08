---
title: Flight model
review: approved
---

# Flight model

The flight model (including ground handling) has been redone and refined over multiple iterations by CanadianCaptainMoustache.

- decreased nosewheel steering sensitivity at medium to high speeds
- adjusted main gear springiness to stop the rolling back and forth and make it sit slightly nose-low on the ground
- drastically increased wing incidence angle
- removed the invisible spoiler
- added prop drag
- decreased elevator and rudder sensitivity around neutral
- increased fuselage side-area
- added ground friction and crosswind numbers
- increased icing physical impact to 400%
- decreased overall rudder maximum effectiveness
- increased elevator trim effectiveness
- decreased gyro stability around all axes
- doubled flap lift
- added 40% to flap drag
- decreased blade angle on the ground to minimize aircraft rolling at idle and quasi-simulate beta range

CCM explains:

> This leads to a Twotter that looks and handles much more like the real thing. Expect that iconic DHC nose-down on approach. Expect massive amounts of both lift and drag beyond 20 degrees of flap. The plane will also handle far better on the ground, both in the taxi and on takeoff. I’m by no means claiming this is the be-all, perfect flight model for this plane and it will likely evolve some but for now we’re hitting book numbers in most cases within 1-2% and the plane is a sheer joy to fly.

The Captain has since contributed several rounds of refinements to the flight model, based on Twotter driver and user feedback.

## Known issues

One important feature is currently missing from the flight model: a mechanical linkage between the flaps and a dedicated trim tab compensates for the extra lift those barndoors create. Currently, that compensation is partially baked into the flight model. I prefer to actually simulate the mechanical linkage, I *just* have to convince CCM that's the way to go.
