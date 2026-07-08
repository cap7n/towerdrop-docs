# Economy & Gold

Gold is the currency that connects the assault to the between-wave shop. Kills drop coins; coins fund upgrades in the [Tab shop](tower.md) and the [six elemental trees](items.md).

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
