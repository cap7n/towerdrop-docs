# Decision Log

Decisions worth not re-litigating, **with the why**. When a real decision is made, add it here.

## Game & scope

- **Name: "Tower Drop."** The in-game title and app name. ("TowerDrop" one-word is used for this wiki / repo.)
- **Enemy count target: 200–250, smooth on most machines.** Not thousands. A modest cap preserves the feel-rich per-enemy effects (fire, ragdoll, shell pop-off) a massive-horde architecture would force us to cut. See [Performance](../tech/performance.md).
- **Base-game fun before progression cleanup.** The messy upgrade/economy systems are parked, not rejected. See [Design Pillars](../pillars.md).

## Combat & feedback

- **Damage numbers removed.** Both a taste call (physical feedback over floating UI) and a perf win (they were a top per-frame allocator).
- **On-enemy HP bars + floating status text removed.** The body-tint hit-flash is the status read instead. Ragdoll deaths carry the "it died" feedback physically.
- **Recipe / auto-combine system fully deleted.** Not parked, gone. Older notes about "~40 parked combos" or an "adjacency-combine minigame" are stale. The enemy **carry** system was kept.

## Difficulty & generator (locked 2026-06-25)

- **Spider HP cap = 15.** A maxed rock can't one-shot a late spider, but frozen enemies take 2×, so **frost is the setup tool** that brings them back into one-shot range. Keep `frozen` at 2×-all (it's load-bearing for this), don't scope it to rock-only yet.
- **Breakable pure-shield.** Rock chips it, elements bypass. Introduced ~wave 15 once players have a few wizards; this is "rock immunity" reintroduced in breakable form.
- **Budget buys variety + powerups, not raw HP.** The endless generator spends its threat budget on elemental immunities and shields, and assigns resistances deliberately (not randomly).
- **Snowball killed by removing always-on TAB passives.** Spikes + poison spikes + HP regen were removed from the TAB tree (systems kept for re-wire as costed spells) because they made late runs unkillable.

## Enemies

- **Keep the monolithic `enemy.gd` for now**, with per-type `@export` flags. Agreed future split into Core + Locomotion/Attack/Abilities components once ~3–4 types exist (the component system is now built and opt-in; migration is pending A/B parity).
- **Spider collision = fitted sphere**, not a tall capsule. `walk_climb_radius` / `web_anchor_radius` tune climb-sit depth.
- **Spider gait is a baked walk/climb animation**, not live IK. An earlier analytic 2-bone IK approach (worked around Blender IK not running headless) was superseded; see [Technical Solutions](../tech/solutions.md) for that history.

## Rendering & effects

- **SDFGI dropped** for the day/night cycle.
- **Menus keep the heavy systems dormant** via an autoload guard (`current_scene is Node3D`): fluid/fog/lights/day-night only run in real levels, not the 3D menu backdrop.
- **Screen FX in one shader, one pass**: adding chromatic aberration / grain means adding a block in the same fragment shader, not stacking full-screen overlays (which would each overwrite the previous).
- **Spider fire uses local coordinates**, not world, so the flame rides the moving body. The process material is baked externally/shared so the bake sticks.

## Art

- **No outlines.** Tried and disliked. Stylized faceted-flat / soft-matte instead. Toon-shader experiments are parked, not committed.
- **Full art pass waits for real models**: don't over-tune placeholder materials.

## Workflow

- **Hybrid: code for systems, scenes/resources for tunable visuals** so values can be felt in the editor. Don't edit a scene/resource from tooling while it's open in the editor (it clobbers on save).
- **This wiki is the single point of truth** for planning, replacing scattered `.md`/HTML notes, mirroring the OTR project's approach.
