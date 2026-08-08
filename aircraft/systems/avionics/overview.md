---
title: Overview
review: approved
---

# Avionics Overview

- Removed the button clicks on the soft buttons of GNS 430/530, KAP 140,
  KT 76C and KR 87. The volume knobs only click on their Off detent.

## GNS 530/430

The GNS units shipped with the mod are exclusive to it. My changes
should not affect GNS units in any other aircraft. The changes are
discussed [here](gns.md).

## KR 87 (ADF)

I forked the KR 87 mainly to enable persistence for its frequencies,
to add an LCD ghost to the display, and to fix a tiny bug that has ben
bothering me forever: changing frequencies or modes worked while the
unit was off, as if it was a mechanical unit.

## L:vars

**R** is read-only state; **R/W** can be set.

| L:var | R/W | Meaning |
|---|---|---|
| `DHC6_ALT_ALERT` | R | Altitude alert state |
| `DHC6_ALT_ALERT_LIGHT` | R | Altitude alert lamp |
