# Items & Elements

The offensive heart of TowerDrop. This is the **active big build** — most current design work flows through it.

## Six damage-type elements

Items are organized into **six elements**, each a damage type with its own identity and its own **per-wizard upgrade tree**:

| Element | Identity |
|---|---|
| **Rock** | Raw impact / crush; also the home of tower-repair and base-spell utility. |
| **Fire** | Ignition and damage-over-time; ignites oil. |
| **Electric** | Chain lightning between enemies. |
| **Poison** | Damage-over-time that can **spread on death** to nearby enemies. |
| **Frost** | Slow / freeze; a frost combo keeps the one-shot on capped-HP spiders. |
| **Lure** | A **beacon that pulls** enemies — crowd control by redirection, not damage. |

Each element has an **upgrade tree** designed in the HTML **skill-tree maker** tool and mirrored into the in-game shop. All six elements are built; **poison spreading-death** and the **lure beacon-pull** are live. What remains is a **tuning pass** — current numbers are a first-pass and largely untested.

## The wizards

Elements are delivered through **per-wizard summons**, each wizard configured for its element. The upgrade trees are branched (sub-roles + ultimates) and include the base-defense spells (spikes, repair) folded in as "rock spells."

## Tooling: the skill-tree maker

The trees are authored in `Tools/skill_tree_maker.html` — a standalone browser tool that stays **in sync** with the in-game shop (it has a "↺ Baked" button to reflect the committed tree data). This is the design surface for the trees before they're wired.

!!! warning "Wiring status varies by node"
    Not every tree node is effect-wired yet. Some node ids are live in the shop; others are **tree-data-only** (they render and cost points but don't yet apply an effect) and need a wiring pass. **Ultimates are currently tree-data-only and not gated.** Check the current shop script before assuming a node does something.

## Damage typing

There is a **damage-typing system** so elements interact with enemy resistances/immunities correctly (see [Enemies](enemies.md) — the wave generator assigns resistances deliberately rather than randomly).

## Related

- [The Cart](cart.md) — how items are delivered.
- [Combat, Status & Feedback](../systems/combat.md) — the status framework the elements drive.
- [Ultimates & Spellbook](../systems/ultimates.md) — the tower-coat ultimates (oil/frost/slime).
