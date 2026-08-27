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

One line per system: the pill is the state, the link is the detail. **When you change a system, change its line.**

The division of labour: **this wiki describes the game in the present tense** — what exists in the build right now. Anything future tense (planned, pending, next) lives on the **Ingenui Tasks** board and nowhere else; no task numbers on this page, ever. Finished work lands in the nightly done-log, which is the cue to update a line here. The **[Backlog](project/backlog.md)** holds design sketches and the **[Decision Log](project/decisions.md)** holds the *why*.

### The Game

| System | State | Where it stands |
|---|---|---|
| [Wave Loop](game/waves.md) | <span class="pill done">DONE</span> | Telegraph → assault → rest, signal-flare sector telegraph, auto-next option. |
| [Curated Waves 1–50](game/waves-curated.md) | <span class="pill done">DONE</span> | Waves 1–10 curated (Queen at 10); 11+ come from the threat-budget generator. |
| [The Cart](game/cart.md) | <span class="pill done">DONE</span> | Tap-toss, hold-pour, drag-reorder, scroll-select, right-click cannon shot. |
| [Tower & Base Defenses](game/tower.md) | <span class="pill wip">WIP</span> | Wall MATERIAL ladder live (2026-08-27): runs start on the meta tree's `wall_material` tier — Sticks/Wood/Braced Wood/Rock/Reinforced Rock/Metal (1-20 HP/brick, Sticks free at Lv0, colour-coded for now, looks = task #169; naked-core tier cut same day). Climbers hug the core where no brick stands. Click-to-repair live (1g/HP flat). Spike ring exists but has no spell; its Rock-tree nodes buyable-but-inert. |
| [Items & Elements](game/items.md) | <span class="pill wip">WIP</span> | Trees REBUILT in the team's skill-tree maker and synced into the game 2026-08-17 (`sync_trees.py`, tool = source of truth); every element now has 3 spells + ultimate + dials authored, ~67 new nodes await their effects (board #138-#153). |
| [Enemies](game/enemies.md) | <span class="pill wip">WIP</span> | Composed architecture (core + components). Spider, snail, pillbug, queen, egg live. Turtle/fly/worm exist as static models only. |
| [Bosses & The Run Arc](game/bosses.md) | <span class="pill wip">WIP</span> | Spider Queen v1 lives at wave 10, playtested; she has no reward drop. |
| [Economy & Gold](game/economy.md) | <span class="pill done">DONE</span> | HP-scaled gold curve; the balance logger records every run. |
| Tutorial (picture cards) | <span class="pill wip">WIP</span> | Trigger-fired picture cards + hotkey overlay, tutorial runs only (rebuilt 2026-07-30). |

### Systems

| System | State | Where it stands |
|---|---|---|
| [Combat, Status & Feedback](systems/combat.md) | <span class="pill done">DONE</span> | StatusDB + six damage types, damage numbers, ragdolls, chain lightning, cloud pools. |
| [Progression](systems/progression.md) | <span class="pill wip">WIP</span> | Save system complete (3 profiles + run checkpoint); achievements v1 + compendium live. Ultimates are not achievement-gated. |
| [Ultimates & Spellbook](systems/ultimates.md) | <span class="pill wip">WIP</span> | Book UI live; oil, frost, slime, meteors, living stone castable. FireWave, Arc Storm and Black-hole exist only as wip book entries. Flat 15g cast cost dies when the mana layer lands. |
| [Mana & The Grinder](systems/mana.md) | <span class="pill todo">Todo</span> | DESIGN LOCKED 2026-08-28: gold builds, mana casts. Capped bar fed by kills; spells and ultimates cost mana, not gold; Lucky Coin removed, its grinder model becomes the Mana Grinder artifact (boosts gain/cap; base gain exists without it). Killed by tester data: nonstop repair-spell casting + a 15k Lucky Coin wave. |
| [Atmosphere & VFX](systems/atmosphere.md) | <span class="pill done">DONE</span> | Day/night, ground-fluid v2, fire-is-fluid, depth fog, living grass. |
| [Artifacts & Relics](systems/artifacts.md) | <span class="pill done">DONE</span> | Bag + placeable artifacts + after-every-wave 3-card draft. Whole [catalogue](systems/artifact-catalogue.md) verified wired 2026-08-13. |
| [Meta-Progression Stats](systems/meta-progression-stats.md) | <span class="pill wip">WIP</span> | Meta layer LIVE + verified (2026-08-27): shards (1/wave clear, boss +10) spend in the full-screen MetaTreeScreen (game-over splash diamond + main-menu button, tool-authored layout via sync_trees.py). `wall_material` ladder = the run-length governor; first real purchase confirmed in play. Most other node GRANTS still to wire (#150-#152). |

### Tech

| System | State | Where it stands |
|---|---|---|
| [Art Direction](tech/art-direction.md) | <span class="pill done">DONE</span> | Faceted-flat, no outlines; the AgX + contrast/saturation look is locked in the shared environment file. |
| [Performance](tech/performance.md) | <span class="pill risk">RISK</span> | Real cap today is 200–250 enemies; the build ships none of the audited quick wins. |
| [Asset Pipeline](tech/asset-pipeline.md) | <span class="pill done">DONE</span> | Blender (Steam install) → re-runnable GLB export scripts; artifact mount contract documented. |
| [Engine & Tooling](tech/engine.md) | <span class="pill done">DONE</span> | Godot 4.7-stable; vfx_bench (F6) is the test bench; balance + perf loggers. |
| [The Task Bot](tech/wiki-bot.md) | <span class="pill wip">WIP</span> | Nightly done-log bot runs on the box; its log branch is not yet merged into this wiki. |

## How to use this wiki

Start with the **[Design Pillars](pillars.md)**: the taste and constraints everything is designed through. Then **[Wave Loop](game/waves.md)** for the run structure, **[The Cart](game/cart.md)** for the core verb, and **[Items & Elements](game/items.md)** for the elemental system that most active work flows into.

!!! note "The one rule of this wiki"
    **This wiki records decisions, it doesn't replace making them.** If a page starts describing something that hasn't actually been decided or built, mark it clearly (a `!!! warning "Not decided"` box) or move it to the [Backlog](project/backlog.md). Stale certainty is worse than an honest "open question."
