# TowerDrop Design Wiki

**TowerDrop is a stylized tower-defense game.** A lone tower stands in a lush field. Enemies stream in from the surrounding terrain, walk to the tower, and climb it. You defend it from a **cart that rides a circular rail around the tower** — you pick up crafting items, aim, and drop or throw them onto the horde below. Between waves you spend gold on an upgrade shop that branches the tower, the cart, and six elemental damage trees.

Built in **Godot 4.7**.

---

## The pitch in one line

*A tower siege you fight from a cart on a rail — rain rocks, fire, poison, and frost onto the swarm climbing your walls, then upgrade between waves.*

## The core idea

The twist that makes TowerDrop its own thing is the **cart on the rail**. You are not a static set of turrets; you are a moving delivery system. Every defensive action is *"get the right item, get to the right spot on the rail, aim, and release."* The tension is positional and about timing — being on the wrong arc of the circle when a wave breaks on the far side is the core failure you're always managing.

On top of that sits **elemental crafting**: items resolve into six damage-type elements (rock, fire, electric, poison, frost, lure), each with its own upgrade tree bought at the between-wave shop.

## Where the game is right now

This wiki is the living record. For the detailed, changing task list see the **[Backlog](project/backlog.md)**; for *why* things are the way they are, the **[Decision Log](project/decisions.md)**.

| Area | Status |
|---|---|
| Cart on circular rail (theta-based, drag-to-place) | ✅ Working |
| Cart drop / toss / pour + aimed throw | ✅ Working (throw in testing) |
| Directional wave loop (telegraph → assault → rest) | ✅ Working |
| Signal-flare wave telegraph | ✅ Working |
| Six elemental damage trees (rock/fire/electric/poison/frost/lure) | ✅ Built; tuning pass pending |
| Tower & Cart (Tab) upgrade shop | ✅ Working |
| Enemies: spider (IK walk + climb) | ✅ Working |
| Enemies: pillbug (curl-roll + charge-bonk-stun) | ✅ Done; not yet wired into waves |
| Enemies: snail (shell armor) | 🧪 Wired (~wave 8), polish pending |
| Endless wave generator (wave 11+, threat budget + powerups) | ✅ Built |
| Ultimates: oil / frost / slime tower coats + spellbook | ✅ Working |
| Atmosphere: fire, oil fluid, day/night, blood/dissolve | ✅ Built |
| Artifact / relic drops | 🧪 Scaffold |
| New modeler tower + brick-shell MultiMesh | ✅ Imported |
| Guided intro tutorial | ⚠️ Built, stopgap, untested |
| Performance (target: smooth at 200–250 enemies) | 📋 Phase 0 quick wins not yet applied |

## How to use this wiki

Start with the **[Design Pillars](pillars.md)** — the taste and constraints everything is designed through. Then **[Wave Loop](game/waves.md)** for the run structure, **[The Cart](game/cart.md)** for the core verb, and **[Items & Elements](game/items.md)** for the elemental system that most active work flows into.

!!! note "The one rule of this wiki"
    **This wiki records decisions, it doesn't replace making them.** If a page starts describing something that hasn't actually been decided or built, mark it clearly (a `!!! warning "Not decided"` box) or move it to the [Backlog](project/backlog.md). Stale certainty is worse than an honest "open question."
