# Wave Loop

The run is a rhythm of **directional waves** with a clear tell, an assault, and a breather, chosen after an earlier "continuous swell" model failed to give the player readable beats.

## The beat: telegraph → assault → rest

1. **Telegraph**: the game announces *which sector* the next wave comes from. A **signal flare** (a code-built firework, `WaveSignal`, with procedural whistle + boom WAVs) fires over the incoming direction. The player uses this to reposition the cart on the rail before the wave lands.
2. **Assault**: enemies stream in from that sector, cross the terrain ring, reach the tower, and climb.
3. **Rest**: a breather between waves. The **between-wave shop** opens here for spending gold.

!!! note "Why directional, not a ring"
    The main driver was **motion sickness and pacing**: a wave from everywhere at once forces the player to keep circling the tower constantly, which is both nauseating and tiring to sustain. Directional waves mean the cart mostly sits still and only repositions when told to. This is a **base-loop default**, not a hard rule: multi-direction waves, or other reasons to force a relocation, may get added later as a *deliberate* spike in difficulty, not as the everyday rhythm.

## HP and size ramp by run time

Enemy toughness scales with **run time**, but the HP scaling is **capped** so enemies don't turn into flat damage sponges (see the [spider HP cap](enemies.md)). Toughness reads as more than a number, though: enemy **size also scales up** with run time, so late-run spiders look and feel bigger and tougher even where their HP has already hit its ceiling. This keeps early waves crisp and late waves threatening without hand-authoring every wave's stats.

## Curated waves 1–10, generated 11+

- **Waves 1–10 are hand-curated** for a designed opening (currently spiders-only, reverted from a mixed set while other enemy types stabilize).
- **Wave 11 onward is generated** by an endless **threat-budget generator** built from wave-50 stress logs. The budget buys **variety and powerups, not raw HP**: see the [Enemies](enemies.md) page for the generator, the spider HP cap, elemental immunities, and breakable pure-shields.

## Related

- [Enemies](enemies.md): the wave generator, powerups, resistances.
- [The Cart](cart.md): what you do during the assault.
- [Economy & Gold](economy.md): what the rest phase funds.

!!! warning "Living values"
    Specific pool sizes, tick intervals, and delays are tuning values that move often. Treat any number here as a snapshot and confirm against the current scene/`WaveManager` before relying on it.
