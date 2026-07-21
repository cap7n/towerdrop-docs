# Economy & Gold

Gold is the currency that connects the assault to the between-wave shop. Kills drop coins; coins fund upgrades in the [Tab shop](tower.md) and the [six elemental trees](items.md).

## What a kill pays (HP-scaled, 2026-07-21)

Gold scales with the victim's **max HP**, so tanky kills pay like jackpots and run income ramps with the HP curve automatically:

- **1 HP = 1 gold**, then **+2 gold per HP** up to 4 HP (2 → 3, 3 → 5, 4 → 7),
- stepping up to **+3 gold per HP from 5 HP** (5 → 10, 6 → 13, … a 15-HP capped spider pays **40 gold**).

The payout arrives as **physical coins** — a big kill visibly rains gold onto the pile, while wave-1 fodder (which rounds to 1 HP) stays at 1 gold per kill by design. Formula lives in `enemy.gd` (`coin_payout`). Elites (snails, pillbugs) inherit the curve, which deliberately makes them priority targets. Prices intentionally did **not** come down with this change: income is the lever, and hoarding a big pile stays part of the game.

## Gold coins & the coin pile

- A dropped coin is a **`GoldCoin` RigidBody3D**, but only while it's falling. It arcs out from the kill and settles.
- The moment it settles, it's **absorbed into a MultiMesh + heightmap coin pile** and its physics node is freed. This is what lets thousands of coins accumulate visually with no frame-rate cost.
- The pile is linked to `Globals.gold_changed`: spending gold (negative deltas) visibly **drains coins** back off the pile.
- The coin shader is PBR metallic + fresnel rim + a triple-sine sparkle on the normal.

See [Technical Solutions](../tech/solutions.md) for why coins are built this way (the RigidBody-then-MultiMesh split).

## Balance tooling

A **`BalanceLogger` autoload** auto-writes economy/pressure CSVs every run (gold flow, tower HP, incoming enemy HP vs. throughput) plus an on-screen overlay (backtick key). The agreed approach is **feel first, then let the numbers inform tuning**, not the reverse. Logs live under the artifacts folder's `balance_logs/`. See [Engine & Tooling](../tech/engine.md).

## Related

- [The Tower & Base Defenses](tower.md): the Tab shop gold buys into.
- [Items & Elements](items.md): the six elemental trees gold buys into.
- [Roadmap & Priorities](../project/roadmap.md): where economy tuning sits.
