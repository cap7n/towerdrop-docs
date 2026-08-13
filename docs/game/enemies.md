# Enemies

Enemies spawn out in the terrain ring, **walk** to the tower, **climb** the wall, and **attack** it: enemies dig in and attack from a range of heights along the wall, not only the rim.

**Architecture (since 2026-08-09, task #53): every enemy is COMPOSED.** A scene is the shared `enemy.gd` core (health, statuses + their body VFX, powerups, carry, size roll, coins, pooling) plus component child nodes from `Characters/Enemies/Components/`: exactly one **Locomotion** (how it moves — `ground_climb.gd` is the reference), one **Attack** (what it does at the tower — `dig_in_bite.gd`), and any number of **Abilities** (web shooter, shell armor, curl, death presentations...). The old inline WALKING/CLIMBING/ATTACKING state machine and the per-type `extends Enemy` subclasses are deleted; a new enemy type is a new scene composing existing components plus at most one new small component.

## Roster

| Enemy | State |
|---|---|
| **Spider** | ✅ Live (`Spider_v2.tscn`, composed). Baked walk/climb animation, speed-matched gait, climbs belly-to-wall, far-field sprint. GroundClimb + DigInBite + WebShooter + RagdollDeath. The primary enemy. |
| **Pillbug** | ✅ Live in waves 4+ (`Pillbug_v2.tscn`, composed). Boneless curl-morph roll; gray gradient shell shader. **Ability:** orbits at 16–30m, every 5–10s charges in and bonks the tower, then flips onto its back and struggles stunned ~3s (immune while curled; any hit cracks it open). PillbugOrbit + CurlArmor + SinkDeath. |
| **Snail** | ✅ Live in waves 6+ (`Snail_v2.tscn`, composed). Blender rig + figure-8 crawl loop. **Shell-armor mechanic**: a 1-HP shell gates the body. SnailClimb + HeightScaledBite + ShellArmor + SquishDeath. Polish pending: UV/texture, progressive shell fracture. |
| **Spider Queen** | ✅ Live (wave 10 boss). OrbitThenClimb + BossShield + SummonBrood + LayEggs + WebArtifacts; eggs are their own composed enemy (EggHatch + BurstDeath). See [Bosses](bosses.md). |

Fly/worm/turtle exist as model-only scenes (no components yet = inert), behavior backlogged.

**Deleted enemy designs (2026-08-09, kept here so the ideas survive the code):** four never-spawned `extends Enemy` subclasses were removed with the legacy path; their scripts live in git history before that date. **Termite**: fragile fast swarm that chews the wall far quicker than anything (fast bite cadence, digs in low, no elemental resist) — punishes ignoring the base. **Bombardier**: acid beetle, resists poison; on death bursts a corrosive cloud that corrodes the TOWER if it dies dug-in — kill it out on the field or pay. **Carrier**: tanky pack-mule that always hauls an item, revealed on death (pure reuse of the core carry system + bonus HP). **Centipede**: segmented head-plus-trail body where damage chews segments off the tail (length-as-health, trail-follower along breadcrumbs); the trail logic was real work — rebuild as a Locomotion when the model exists.

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
