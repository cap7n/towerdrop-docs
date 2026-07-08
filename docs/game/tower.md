# The Tower & Base Defenses

The tower is the thing you protect. Enemies walk to it, **climb its wall**, reach the rim, and attack it. If it falls, the run ends.

## The Tower & Cart ("Tab") shop

The base-defense upgrades live in the **Tab shop** (Tower & Cart). It's the between-wave spend that hardens the tower itself and the cart, as distinct from the six elemental *offense* trees. Tab currently covers:

- **Base HP**.
- **Cart** speed and inventory size.

Built on a `ShopGraph` (+ `SpikeRing` for the spikes system).

!!! warning "Spikes & regen were pulled from the TAB tree (snowball fix, 2026-06-25)"
    The C4 wave-39 marathon went unkillable past wave 10 because always-on **spikes + poison spikes + HP regen** insta-killed everything and healed the tower back faster than it lost HP. Those were **removed from the TAB tree**; the systems are kept for re-wire. The plan is to re-home them as **costed, upgradeable spells**: spikes become a **Rock-wizard spell** (re-point to `_apply("spiks"/"posioned_spikes")` + `SpikeRing`), regen becomes an upgradeable spell (`tower_core.regen_amount`). Until that wiring lands, the tower has no always-on spikes or regen. See the [Backlog](../project/backlog.md).

## Architecture

- The tower is **packed into a `GameTower` scene**.
- References are **group-based** (not fixed node paths), so the tower can be reparented without breaking wiring.
- A `spikes_anchor` marker positions the spike ring.

!!! note "Recipe system is gone"
    The old auto-combine / recipe / combo system was **fully removed**. Any older note describing "~40 parked combos" or an "adjacency-combine minigame" is stale. The enemy **carry** system (enemies hauling artifacts) was kept.

## The new modeler tower + brick shell

A new tower model was delivered by the 3D modeler and extracted into `res://Imported/NewTower/`. Its decorative **brick shell is rebuilt as a MultiMesh** (one draw call) from the modeler's own geometry-node layout: 110 accent bricks.

- The shell scene is `Tower/tower_brick_shell.tscn` (`TowerBrickShell`, extends `MultiMeshInstance3D`).
- **Bricks are individually movable at runtime** and carry a **damage-state system**: `set_brick_state()`, `damage_brick()` (advances a brick through intact → damaged → gone), `pop_brick()`, `reset_bricks()`, `nearest_brick(point)`.
- Each brick's damage state doubles as its "HP", event-driven, effectively free at our enemy counts.
- **Placeholder damage colors** tint via MultiMesh per-instance colors (orange → red) until real damaged-brick models are dropped into the `damage_scenes` array. An inspector `preview_damage` toggle splatters test damage.

This is the foundation for the planned **tower-damage "dig-in" rework** (enemies chewing a specific spot rather than generic tower HP loss). See the [Backlog](../project/backlog.md) and [Asset Pipeline](../tech/asset-pipeline.md).

## Related

- [Enemies](enemies.md): how climbers reach and attack the rim.
- [Ultimates & Spellbook](../systems/ultimates.md): oil/frost/slime coats that also affect dug-in attackers.
