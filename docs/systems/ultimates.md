# Ultimates & Spellbook

A right-edge **spellbook UI** (`UltimateBook` autoload) lets the player cast tower-coating **ultimates** — big, screen-changing effects that coat the tower and punish everything climbing it.

## The three coats

The player **casts each ultimate from the Spellbook for 15 gold, 10 seconds each**. Each coats the tower wall in a fluid that affects climbers — **including dug-in attackers** at the rim (`FluidField._apply_coat_effects` reaches them, not just surface climbers).

| Ultimate | Wizard | Effect |
|---|---|---|
| **Oil** | — | Coats the wall; enemies enter a slowed "bubble mode" (slowed ~3×, volume up ~3×). Oil **ignites** into a fire wave when hit by fire (`ignite_world`). |
| **Frost** | Frost | Freezes the tower in an **ice-rock crust** (6 reshaded rocks as a MultiMesh climbing base→top). Frozen enemies: speed 0 and take **2× all damage**. |
| **Slime** | Poison | A thick green translucent **jiggly mucus** coat (noise blobs, time-driven wobble, crawling flow, wet fresnel rim) that **slows + poisons** climbers. |

All three mirror the same oil/ice coat system in `FluidField`. (Dev-only fluid-tuning keys `O`/`I`/`P` still exist behind `DEBUG_TOOLS`, but the player-facing cast is the Spellbook.)

!!! warning "Open: final home for ultimates"
    Right now ultimates are always-available from the Spellbook. The intended end-state is to **gate them deep in each element's wizard tree** rather than an always-on spellbook — undecided/unbuilt. See the [Backlog](../project/backlog.md).

## Shaders

- `ice.gdshader` — fresnel-rim ice crust. (Planned extension: ice wall-coat + steam via a `FrostFx`.)
- Slime uses noise-blob geometry with a wobble driven by `TIME` and a wet rim.

## Related

- [The Tower & Base Defenses](../game/tower.md) — what's being coated.
- [Atmosphere & VFX](atmosphere.md) — the oil fluid system and fire that ignites it.
- [Items & Elements](../game/items.md) — the per-element trees; ultimates currently live as tree data and are **not yet gated**.
