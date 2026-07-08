# Enemies

Enemies spawn out in the terrain ring, **walk** to the tower, **climb** the wall, and **attack** it: enemies dig in and attack from a range of heights along the wall, not only the rim. The behavior lives on a shared `enemy.gd` (`class_name Enemy`) state machine: WALKING → CLIMBING → ATTACKING.

## Roster

| Enemy | State |
|---|---|
| **Spider** | ✅ Live. Baked walk/climb animation, speed-matched gait, climbs belly-to-wall. The primary enemy. Scene `Spider.tscn`. |
| **Pillbug** | ✅ Done + feels good. Boneless curl-morph roll (faces travel, rolls head-over-tail); gray gradient shell shader. **Ability:** orbits at 13–25m, every 5–10s charges in and bonks the tower, then flips onto its back and struggles stunned ~3s (immune while curled; a blast cracks it open). Not yet wired into waves. |
| **Snail** | 🧪 Wired (appears ~wave 8). Blender rig + figure-8 crawl loop. **Shell-armor mechanic**: a 1-HP shell gates the body. Spider-only behaviors (web, ragdoll) gated off. Polish pending: UV/texture, progressive shell fracture, squish-death. |

More subclasses exist as thin `extends Enemy` scripts (termite, bombardier, carrier, centipede) awaiting models/scenes; fly/worm/turtle exist as Blender source, not yet wired. See the [Backlog](../project/backlog.md).

New enemy types are added as per-type `@export` flags on the shared `enemy.gd` rather than new classes for now; see the [Decision Log](../project/decisions.md) for why, and [Technical Solutions](../tech/solutions.md) for implementation write-ups like the spider's gait.

## Endless wave generator + powerups

From wave 11 on, an **endless threat-budget generator** (built from wave-50 stress logs) composes waves. Key rules:

- **Curated waves 1–10 are kept**; the generator only drives 11+.
- **Spider HP is capped at 15** so a frost combo can still one-shot.
- The threat budget buys **variety and powerups, not raw HP**. Powerups include:
    - **Elemental immunity** to a specific damage type.
    - A **breakable "pure shield"** (via `enemy.set_pure_shield`).
- **Resistances are generator-assigned**, deliberate, not random.

This ties into the [Items & Elements](items.md) damage-typing system: an enemy immune to fire must be answered with another element.

## Enemy carry system

Enemies can **carry artifacts**; the carry system was kept when the recipe system was deleted. See [Artifacts & Relics](../systems/artifacts.md).

## Related

- [Wave Loop](waves.md): how enemies arrive (directional telegraph, HP ramp).
- [Combat, Status & Feedback](../systems/combat.md): how they die (ragdoll, chain lightning, corpses).
- [Performance](../tech/performance.md): the 200–250 target and why it's comfortable.
