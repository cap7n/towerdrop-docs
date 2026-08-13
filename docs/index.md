# TowerDrop Design Wiki

**TowerDrop is a stylized tower-defense game.** A lone tower stands in a lush field. Enemies stream in from the surrounding terrain, walk to the tower, and climb it. You defend it from a **cart that rides a circular rail around the tower**: you pick up crafting items, aim, and drop or throw them onto the horde below. Between waves you spend gold on an upgrade shop that branches the tower, the cart, and six elemental damage trees.

Built in **Godot 4.7**.

---

## The pitch in one line

*A tower siege you fight from a cart on a rail: rain rocks, fire, poison, and frost onto the swarm climbing your walls, then upgrade between waves.*

## The core idea

The twist that makes TowerDrop its own thing is the **cart on the rail**. You are not a static set of turrets; you are a moving delivery system. Every defensive action is *"get the right item, get to the right spot on the rail, aim, and release."* The tension is positional and about timing: being on the wrong arc of the circle when a wave breaks on the far side is the core failure you're always managing.

On top of that sits **elemental crafting**: items resolve into six damage-type elements (rock, fire, electric, poison, frost, lure), each with its own upgrade tree bought at the between-wave shop.

## State of the game

One line per system: the pill is the state, the link is the detail. **When you change a system, change its line** — this table is the orientation point the rest of the wiki hangs off. Day-to-day tasks live on the **Ingenui Tasks** board (Tower Drop project); finished work lands in the nightly done-log; the **[Backlog](project/backlog.md)** holds design sketches and the **[Decision Log](project/decisions.md)** holds the *why*.

### The Game

| System | State | Where it stands |
|---|---|---|
| [Wave Loop](game/waves.md) | <span class="pill done">DONE</span> | Telegraph → assault → rest, signal-flare sector telegraph, auto-next option. Pacing pass landed. |
| [Curated Waves 1–50](game/waves-curated.md) | <span class="pill check">CHECK</span> | Waves 1–10 curated (Queen at 10), 11+ from the threat-budget generator. Numbers ride the tuning pass. |
| [The Cart](game/cart.md) | <span class="pill done">DONE</span> | Tap-toss, hold-pour, drag-reorder, scroll-select, right-click cannon shot. Next: per-element impact identities. |
| [Tower & Base Defenses](game/tower.md) | <span class="pill wip">WIP</span> | Brick armour (10 HP/brick) + click-to-repair live. Spike ring waits on its spell (board #5). |
| [Items & Elements](game/items.md) | <span class="pill wip">WIP</span> | All six trees built; coverage audit 2026-08-13 confirmed every buyable node is wired or board-tracked. Now: the tuning pass. |
| [Enemies](game/enemies.md) | <span class="pill wip">WIP</span> | Composed architecture (core + components). Spider, snail, pillbug, queen, egg live. Turtle/fly/worm models wait on behaviours. |
| [Bosses & The Run Arc](game/bosses.md) | <span class="pill wip">WIP</span> | Spider Queen v1 at wave 10, playtested. Tuning + the Regicide reward pick still open. |
| [Economy & Gold](game/economy.md) | <span class="pill done">DONE</span> | HP-scaled gold curve; the balance logger records every run. |
| Tutorial (picture cards) | <span class="pill wip">WIP</span> | Rebuilt 2026-07-30 as trigger-fired picture cards + hotkey overlay. Tutorial-runs-only; refinement parked until post-revamp. |

### Systems

| System | State | Where it stands |
|---|---|---|
| [Combat, Status & Feedback](systems/combat.md) | <span class="pill done">DONE</span> | StatusDB + six damage types, damage numbers, ragdolls, chain lightning, cloud pools. |
| [Progression](systems/progression.md) | <span class="pill wip">WIP</span> | Save system complete (3 profiles + run checkpoint); achievements v1 + compendium live. Deeper ultimate gating open (#26, #30). |
| [Ultimates & Spellbook](systems/ultimates.md) | <span class="pill wip">WIP</span> | Book UI live; oil, frost, slime, meteors, living stone castable. FireWave, Arc Storm, Black-hole still to build (#27, #28, #29). |
| [Atmosphere & VFX](systems/atmosphere.md) | <span class="pill done">DONE</span> | Day/night, ground-fluid v2, fire-is-fluid, depth fog, living grass. Tuning closed for now (#68). |
| [Artifacts & Relics](systems/artifacts.md) | <span class="pill done">DONE</span> | Bag + placeable artifacts + after-every-wave 3-card draft. Whole [catalogue](systems/artifact-catalogue.md) verified wired 2026-08-13. |
| [Meta-Progression Stats](systems/meta-progression-stats.md) | <span class="pill idea">IDEA</span> | Achievements feed it; the meta layer itself is still design. |

### Tech

| System | State | Where it stands |
|---|---|---|
| [Art Direction](tech/art-direction.md) | <span class="pill done">DONE</span> | Faceted-flat, no outlines; the AgX + contrast/saturation look is locked in the shared environment file. |
| [Performance](tech/performance.md) | <span class="pill risk">RISK</span> | Real cap 200–250 enemies; Phase 0 quick wins not applied yet. |
| [Asset Pipeline](tech/asset-pipeline.md) | <span class="pill done">DONE</span> | Blender (Steam install) → re-runnable GLB export scripts; artifact mount contract documented. |
| [Engine & Tooling](tech/engine.md) | <span class="pill done">DONE</span> | Godot 4.7-stable; vfx_bench (F6) is the test bench; balance + perf loggers. |
| [The Task Bot](tech/wiki-bot.md) | <span class="pill wip">WIP</span> | Nightly done-log bot live on the box; `bot/log` branch awaits its first merge, then the log joins the nav. |

## How to use this wiki

Start with the **[Design Pillars](pillars.md)**: the taste and constraints everything is designed through. Then **[Wave Loop](game/waves.md)** for the run structure, **[The Cart](game/cart.md)** for the core verb, and **[Items & Elements](game/items.md)** for the elemental system that most active work flows into.

!!! note "The one rule of this wiki"
    **This wiki records decisions, it doesn't replace making them.** If a page starts describing something that hasn't actually been decided or built, mark it clearly (a `!!! warning "Not decided"` box) or move it to the [Backlog](project/backlog.md). Stale certainty is worse than an honest "open question."
