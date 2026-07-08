# Combat, Status & Feedback

The framework that turns an item landing on an enemy into readable, physical feedback.

## Status framework

A central **`StatusDB`** defines status effects (burn, poison, slow/freeze, etc.) that the six [elements](../game/items.md) apply. Statuses stack and interact through a `CompositeEffect` path so multiple items on one enemy resolve coherently.

## Feedback: physical, not floating

Per the [design pillars](../pillars.md), feedback is deliberately **physical** rather than UI-overlay:

- **Damage numbers were removed** — they were also a top per-frame allocator; killing them was a perf win as much as a taste call.
- **Floating status text / on-enemy HP bars were removed.** The enemy's **body-tint hit-flash** (a grime/feedback shader hue shift) is the status read instead.
- **Spider-corpse ragdoll** — dead spiders tumble physically off the wall rather than vanishing in a burst.

## Signature combat effects

- **Chain lightning** (electric) — arcs between nearby enemies.
- **Cloud pools** — lingering area effects on the ground.
- **Shrapnel** — fragment spread on certain impacts.
- **Fall damage** — enemies knocked off the wall take damage on landing.
- **Poison spreading-death** — a poisoned enemy dying seeds poison to neighbors.
- A reusable **dissolve shader** and a 300-bubble blood effect (see [Atmosphere & VFX](atmosphere.md)).

## Wiring status

The base status effects plus a large set of named crafted effects are wired. As with the trees, **coverage is uneven** — check the current effect wiring before assuming a given item routes through a given status. This is part of the [Items tuning pass](../game/items.md).

## Related

- [Items & Elements](../game/items.md) — what applies these statuses.
- [Atmosphere & VFX](atmosphere.md) — the fire/oil/blood/dissolve visual systems.
- [Performance](../tech/performance.md) — why per-frame allocation (like the old damage numbers) matters.
