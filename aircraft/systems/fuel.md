---
title: Fuel system
review: approved
---

# Fuel system

The mod uses Asobo's modern, modular fuel system which simulates fuel
lines, pumps, and valves to connect tanks to engines.

The main tanks are under the cabin floor. When the fuel selector
switch is in the BOTH position, the FWD tank feeds the right engine,
the AFT tank the left engine. Setting the fuel selector to FWD or AFT
opens the crossfeed valves, forces both boost pumps (main pump and
emergency pump) on for this tank and disables the boost pumps in the
other tank.

Based on pilot feedback, I removed the boost pump requirement when
operating below 8,000 ft density altitude.

## Wing tanks

The wing tanks cannot feed engines directly. With the fuel pump switch
in the (mislabeled) "Engine" position, fuel is transferred from the
respective wing tank to its main tank: the right wing tank feeds into
the FWD main tank. A float valve pauses the transfer before the target
main tank reaches capacity. If the wing tank is empty and the pump can
no longer draw fuel, the respective wing tank pump failure warning
light will illuminate on the annunciator panel. The "Refuel" position
does nothing.

> [!NOTE]
> The modular fuel system has some limitations that I work around with

	config tricks and custom code. Examples:

    Fuel is fed from the wing tanks into the main tanks using electric
    pumps. For some reason, this doesn't work as expected in the sim, so I
    simulate the electrical pumps using valves, gravity feeds and electrical
    phantom consumers.

    The sim controls the engine-driven pumps based on shaft RPM (prop RPM)
    instead of gas generator RPM and doesn't like it when boost pumps
    and engine-driven provide pressure simultaneously, so custom code has
    to orchestrate the pumps.
