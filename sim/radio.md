---
title: Radio navigation
review: approved
---

# Radio navigation

## Overview

The mod completely replaces the stock sim system. It reads the
currently active nav database (NavBlue, Navigraph, whatever) from the
sim, optionally adds decommissioned NDB stations, and calculates all
signals in custom code.

The code was initially **heavily inspired** by libradio but is not
based on it. The implementation is based on ITU publications and data
created using publicly available tools and data sources.

In order to provide performant terrain lookups over long ranges, the
mod ships a global terrain database in a custom, optimized format that
allows for efficient lookups and only contains exactly the details we
need (terrain elevation and land classes).

All gauges, the autopilot and the TAWS implementation exclusively use
the signals from the custom system.

All calculations use the physical effects appropriate for the specific
frequency ranges. If you're interested, the Radio tab in the EFB shows
many of the details and the code is open source. In this manual, I
will focus on the differences you might see compared to the stock sim
system.

> [!NOTE]
> There is some (defensible) cheating going on:
>
> The regulations are pretty clear that stations must reach their
> published range, but they're not allowed to use more power than
> necessary to achieve that range, in order to avoid interference.
>
> The actual transmitter wattage is **not published.** So I try to
> recreate that regulatory decision for every station individually
> and pick the least powerful equipment that can satisfy the range
> based on the location and terrain around the station.

## LF/MF

An NDB signal is low or medium frequency and mostly travels along the
ground, following the terrain. It doesn't depend on line-of-sight. The
range varies massively based on the conductivity of the terrain the
signal traverses. 

A coastal NDB that reaches its published range inland will have a much
higher range when you approach it over water.

NDB signals are much less reliable than you're used to from the stock
system. They tend to become more reliable as you get closer, but even
up close, the signal can be skewed by coastal refraction, terrain
"bending", or reflection by buildings in urban areas.

For me, this makes ADF navigation a lot more fun. You can't just
follow the needle when it comes alive, you have to consider whether
the direction it's pointing to is plausible, maybe wait a bit for the
needle to settle before you trust it.

Known issue: the ADF radio should react to lightning, but the sim
doesn't provide the necessary data that would be needed to bend the
needle toward a storm cell.

## VHF

The Very High frequency (VHF) band is used by VOR and localizers. They
reach their optimal range when you have line-of-sight to the station.

If terrain is blocking LOS, you can still receive the signal refracted
by the terrain, but it will lower its range.

Localizers are shaped beams that, by regulation, must cover a specific
cone and range.

## UHF

ILS glideslopes and DME signals use ultra-high frequencies.

The DME frequency (VHF) you set in your radio is not actually the real
DME frequency (UHF). To keep pilots from having to dial in yet another
frequency, the DME interrogator uses a frequency that is paired with a
specific VHF frequency.

> [!NOTE]
> If you set your NAV radio to a VOR on 117.40, there are two paths to
> get a distance:
>
> **Garmin avionics** will listen for the station ID (the Morse code) to
> positively identify the VOR station, then they look up the station
> (based on ID and frequency) in their database, get the station’s GPS
> location, and calculate the ground distance based only on the GPS
> coordinates.
>
> A **DME interrogator** is a piece of hardware that is part of your radio
> stack. It determines the actual DME frequencies from the
> NAV radio’s frequency: in our example, 117.40 MHz (VHF) results in
> a request frequency of 1135 Mhz (UHF), the response is received on

	1198 MHz (UHF).
    
    The DME interrogator then sends signals to the ground station (the
    transponder) which responds after a fixed delay. Slant distance is
    then calculated based on the round-trip time.
    
    The Twotter does not simulate that handshake, that would be
    pointless, but it does determine the correct UHF frequency and uses
    it to simulate the propagation of the DME signal, because the
    frequency determines how well the signal traverses terrain,
    which affects the range.

Main difference to the stock sim: VOR signals and their paired DME
signals have different ranges.

The same applies pretty much to the ILS glideslope. Glideslopes have
other fun properties like false lobes that appear above the actual
glideslopes, making a "slam dunk" capture from above way more
exciting.

> [!TIP]
> The false glideslope lobes for a standard 3° approach sit at 6° and 9°,

	and the one at 6° produces reversed guidance.
