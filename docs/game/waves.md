# Wave Loop

The run is a rhythm of **directional waves** with a clear tell, an assault, and a breather, chosen after an earlier "continuous swell" model failed to give the player readable beats.

## The beat: telegraph → assault → rest

1. **Telegraph**: the game announces *which sector* the next wave comes from. A **signal flare** (a code-built firework, `WaveSignal`, with procedural whistle + boom WAVs) fires over the incoming direction. The player uses this to reposition the cart on the rail before the wave lands.
2. **Assault**: enemies stream in from that sector, cross the terrain ring, reach the tower, and climb.
3. **Rest**: a breather between waves. The **between-wave shop** opens here for spending gold.

!!! note "Why directional, not a ring"
    A wave from *everywhere at once* removes the core positional decision. A wave from *somewhere specific* is what makes the cart's position on the rail matter: you can be caught on the wrong arc. The telegraph exists to make that a fair, readable choice rather than a memorization test.

## HP ramp by run time

Enemy toughness scales with **run time**, driven by a size cap that ramps up: the base enemy `max_hp` is low, and the ramp (via `Globals.run_time`) increases effective HP as the run progresses. This keeps early waves crisp and late waves threatening without hand-authoring every wave's stats.

## Curated waves 1–10, generated 11+

- **Waves 1–10 are hand-curated** for a designed opening (currently spiders-only, reverted from a mixed set while other enemy types stabilize).
- **Wave 11 onward is generated** by an endless **threat-budget generator** built from wave-50 stress logs. The budget buys **variety and powerups, not raw HP**: see the [Enemies](enemies.md) page for the generator, the spider HP cap, elemental immunities, and breakable pure-shields.

## Related

- [Enemies](enemies.md): the wave generator, powerups, resistances.
- [The Cart](cart.md): what you do during the assault.
- [Economy & Gold](economy.md): what the rest phase funds.

!!! warning "Living values"
    Specific pool sizes, tick intervals, and delays are tuning values that move often. Treat any number here as a snapshot and confirm against the current scene/`WaveManager` before relying on it.
