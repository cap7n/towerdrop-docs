# Backlog

A living to-do list. Edit freely: tick boxes, add items, reorder.

**Status tags:** <span class="pill done">Done</span> shipped · <span class="pill wip">WIP</span> in progress · <span class="pill todo">Todo</span> not started · <span class="pill idea">Idea</span> undecided · <span class="pill check">Check</span> may already be done.

---

## Playtest build v0.1

Goal: a stable, understandable build that's beatable to ~wave 10.

### Tier 0: build hygiene

- <span class="pill done">Done</span> Bake the ProtonScatter foliage in `level_1` and `menu_world` (kills load-time pop-in); set `force_rebuild_on_load = false`.
- <span class="pill done">Done</span> Disable the snail wave-1 test hook (wave 1 = spiders via curated list; pillbugs W6, snails W8).
- <span class="pill wip">WIP</span> Cheat keys → **DEBUG SANDBOX panel** (F4 opens a button panel instead of dumping gold). `DEBUG_TOOLS` kept ON for testers deliberately. Fold remaining raw keys into the panel before a wider build.
- <span class="pill done">Done</span> Kill debug overlays + `Globals.DEBUG` console spam (only the BalanceLogger per-wave line prints).
- <span class="pill todo">Todo</span> Exclude test scenes (`vfx_bench`, `enemy_bench`, `cart_test`) from the player flow (export filter already excludes them).
- <span class="pill done">Done</span> Visible version/build label on the menu ("Tower Drop" + version string). Bump per build.

### Tier 1: core loop (must work)

- <span class="pill wip">WIP</span> Shops:
    - <span class="pill done">Done</span> **Ultimates castable + home decided** — the SPELLBOOK is the home (decided 2026-07-19; see the book plan under "Shop / trees: Pass 2 wiring"). Oil/Frost/Slime/Meteors castable from the book v1.
    - <span class="pill done">Done</span> **Conjuring-time UI** — DONE (2026-07-19) as a QoL settings toggle ("Conjure timers", off by default): an element-coloured countdown above each conjuring wizard's item. The subtle conjure circle stays the default look.
    - <span class="pill done">Done</span> Per-level costs wired (`wizard_shop_ui._node_cost` uses the `costs[]` array). Rebalance still open.
    - <span class="pill wip">WIP</span> Tooltips / descriptions: hover tooltips wired; copy still needs filling in + spellcheck.
- <span class="pill todo">Todo</span> **Waves beatable to ≥10**: tune size/HP ramp and make sure defenses keep up.
- <span class="pill todo">Todo</span> **Item→effect coverage**: every buyable/usable item must actually do something (no dead items).

### Tier 2: onboarding

- <span class="pill todo">Todo</span> **Tutorial overhaul**: walk the actual systems (cart drop/pour + aimed throw, element trees, base + wizard shops, build/traps, tower ultimates), not just intro hints.
- <span class="pill todo">Todo</span> **Spellbook onboarding** — since unowned spells are hidden (2026-07-19), nothing teaches the book exists. Wanted: a tutorial beat when the FIRST ultimate is bought (point at the right-edge tab / auto-open the book on its new spell).
- <span class="pill todo">Todo</span> **Controls reference** card (move / drop / throw / shop keys), a pause-screen list at minimum.
- <span class="pill todo">Todo</span> **Quick Tutorial Screen** — a fast, single-screen quick-reference (the essentials: move / drop / throw / shop / repair) for players who skip the full guided mode.
- <span class="pill todo">Todo</span> **New-player tutorial** — the dedicated guided new-player tutorial mode (the revamp's [Track 1](revamp.md)): slow, its own thing, walks each system.

### Tier 3: game shell (mostly exists, verify + fill gaps)

- <span class="pill done">Done</span> Start menu, pause menu, game-over, export preset exist → verify end-to-end.
- <span class="pill done">Done</span> Title screen rebuilt: live 3D backdrop (cart orbits tower) + button column + day/night + fog. Settings/Compendium/Profiles buttons are stubs.
- <span class="pill todo">Todo</span> Menu fog still looks a bit weird: revisit `menu_atmosphere` fog tuning.
- <span class="pill done">Done</span> Bake the menu scatter (same as level_1) so the menu loads instant.
- <span class="pill parked">Parked</span> **Win / summary state** — decided 2026-07-19: NO win condition, runs stay endless. A run-summary screen is still wanted someday; parked as an [open question in Decisions](decisions.md).
- <span class="pill todo">Todo</span> Clean **restart** after game-over without relaunching.
- <span class="pill done">Done</span> **Settings** — DONE (2026-07-16): music/sound volume (real audio buses), fullscreen, windowed resolution; persists to `user://settings.cfg`; reachable from the start AND pause menus.
- <span class="pill wip">WIP</span> Tester feedback channel: in-game note (press C) exists; still want a Discord/report link visible.

### Tier 4: looks-finished polish

- <span class="pill done">Done</span> **Wizards replaced (2026-07-12):** the rigged goblin wizard (`Blender/Goblin.glb`) stands in for every shop wizard — Conjure anim looping 24/7 with a 30° gaze-up, the shared `purple` cloth material recolors the whole outfit per element (darkened when specialized), power tier = hat size. More anims (real Idle, hat-tip, ear wiggle) = polish backlog. *(The item cubes for fire/electric/frost/lure items are separate and still placeholder.)*
- <span class="pill done">Done</span> Confirm hit feedback is clear without damage numbers: body hit-flash only.
- <span class="pill todo">Todo</span> Tower HP bar shrink / reposition; HUD readability pass (wave / HP / gold / loadout).
- <span class="pill wip">WIP 2026-07-16</span> Audio: key SFX + music balance. `Sfx` autoload built (UI = flat/centered, world = positional 3D, pitch wobble + throttle, auto-click on every button). Wired: clicks, coins, skill unlock, spider bite, pillbug slam, wall repair (Kenney CC0, `Game Sounds/UsedSounds/`). Still missing: ultimate casts, wave-warning tie-in, item impacts, enemy deaths. *(Volume settings: DONE via the settings menu, 2026-07-16.)*
- <span class="pill todo">Todo</span> **Perf Phase 0 quick wins** (shadows, alloc flood, ragdoll caps) so wave 10 doesn't chug.

---

## Meta-progression

- <span class="pill todo">Todo</span> **Meta-progression is a MUST** — the game needs a persistent meta layer across runs (right now everything resets on game over). Must-have, not a maybe. Settles the long-standing "do relics persist across runs?" question in favour of persistence.
- <span class="pill todo">Todo</span> **Meta unlock of artifacts** — artifacts unlock progressively through meta-progression: start with a small pool and unlock more of the [catalogue](../systems/artifact-catalogue.md) across runs (the meta reward loop). The UNLOCK persists between runs; the per-run draft still draws from your unlocked pool.

## Tower ultimates

- <span class="pill done">Done</span> **Rock: Oil the tower** (oil coat + fire-wave). Key `O` / `L` ignite.
- <span class="pill done">Done</span> **Frost: Freeze the tower** (ice rocks + ice wall coat). Key `I`.
- <span class="pill done">Done</span> **Poison: Slime the tower** (jiggly mucus, slow+poison). Key `P`.
- <span class="pill wip">WIP</span> **Rock ultimate: Meteorite Shower** — v1 CASTABLE (2026-07-19): `MeteorShower` (Levels/Scripts) rains real rock DroppedItems over the incoming sector (near→fog band, sqrt-area spread, drip-spawned, live-item perf cap; all knobs @export — 100/s × 10 s start values need tuning). Wired as a 4th card in the test spellbook. Still open: the rock-tree node for it, wizard-buff inheritance (see Decisions), and the real spellbook home.
- <span class="pill idea">Idea</span> Do Fire / Electric / Lure want their own tower ultimates too? (6-element set)
- <span class="pill todo">Todo</span> Tower ultimates gated **deep in the wizard tree** rather than the always-on spellbook.

## Bigger features (later)

- <span class="pill todo">Todo</span> **Save system (run checkpoints)** — feasibility checked 2026-07-21: cleanly doable. Model: **checkpoint at REST** (between waves — no live enemies or in-flight items, so the transient mess never gets saved). Auto-save on entering rest + on quit-to-menu; delete the file on game over; menu gets a **Continue** button when a save exists. One `SaveSystem` autoload writing versioned JSON to `user://save_run.json`. The full state surface, all confirmed serializable:
    - *Globals*: gold, run_time, unlocked/discovered items (by name), guaranteed spawns, damage bonuses.
    - *WaveManager*: wave index (resume into that wave's rest), kills + kills-by-type (stats).
    - *Wizards*: hired count is TAB-tree driven (`turret_stack.place_next_wizard()` × N restores the bodies), then per wizard: `element`, `specialized`, `_tree_levels` dict, gold-invested stat. Tree effects are query-based (`node_level()` reads), so restoring the dict restores the behaviour.
    - *TAB shop*: `ShopGraph._count` (id → levels) + re-apply hooks for buy-time side effects (spike ring, item unlocks).
    - *Artifacts*: `_collected` counts + per-placed `{id, transform, linked_brick}` — re-mount through the normal commit path so effects/brick links rewire themselves.
    - *Tower*: core HP current/max stored RAW (avoids double-applying upgrade bonuses) + per-brick armour state/bonus arrays (shell API exists).
    - *Cart*: held item names (+ selected index).
    - Risk spots: purchases with buy-time side effects need idempotent re-apply; load order (level ready → apply state deferred). Both manageable. NOT in v1: mid-assault saves, cross-run meta progression (separate decision).
- <span class="pill todo">Todo</span> **Menu tower wears your saved artifacts** (parked idea, Cap7n 2026-07-21) — while the cart orbits the menu tower, the player sees THEIR artifacts from the save file mounted on it, scrolling by. Implementation sketch: the save file already stores placed artifacts as `{id, transform, linked_brick}` relative to the tower axis — `menu_world.tscn` (the 3D backdrop) reads `user://save_run.json` on menu load and instances each artifact's **visual scene only** (no `mounted=true`, no effects — the autoload level-guard already keeps gameplay dormant in menus) at the saved transform on the menu tower. Needs: the menu tower to share the game tower's dimensions (it does — same model), a small "spawn visuals from save" helper in the menu world script, and skipping brick-linked artifacts if the menu tower has no brick shell (or just mounting them at the transform anyway — it's cosmetic). Depends on: the save system above.

- <span class="pill wip">WIP</span> **Tower-damage rework**: spiders dig in & attack at a random wall height. Pass 1 done (band 40→95% of top, `attack_height_min/max_frac`). TODO: HP/damage rebalance.
- <span class="pill done">Done</span> **Visible tower damage** — DONE (2026-07-16) via the brick-ARMOUR rework: the 110-brick shell is a real armour layer (local coverage radius) that soaks attacks BEFORE the HP bar; bricks tint, crack and tumble off (physical debris) where enemies dig in; the repair tool rebuilds them. See [The Tower](../game/tower.md).
- <span class="pill todo">Todo</span> **Buildable defenses**: satellite towers + traps (TAB-shop → build tray → click-to-place). Iter-1 exists but free / no purchase.
- <span class="pill todo">Todo</span> **Traps**: more placeable ground hazards (caltrops / grinder / conveyor / chain); new plan: buy at a wizard then throw into place (reuse aimed-throw).
- <span class="pill wip">WIP</span> **More enemy types**: scripts done (thin `extends Enemy` subclasses); each needs a model + scene:
    - <span class="pill done">Done</span> **Pillbug**: done + feels good (curl-morph roll, charge-bonk-stun ability). Polish: real leg-wiggle shape key, UV texture, wire into waves. Redesign: replace slow float-drop with a thrown "shot".
    - <span class="pill todo">Todo</span> **Termite**: fast ~1HP swarm, chews wall fast.
    - <span class="pill todo">Todo</span> **Bombardier**: death-cloud corrodes the tower; resists poison.
    - <span class="pill todo">Todo</span> **Carrier**: always hauls an item, tankier.
    - <span class="pill todo">Todo</span> **Centipede**: head tows follower segments; first pass exists. TODO: per-segment colliders, mid-split, status propagation.
    - <span class="pill done">Done</span> **Snail**: in-game (shell-armour gates the body). Polish parked below.
    - <span class="pill wip">WIP</span> **Turtle / Fly / Worm** (modeler's 3 creatures): models wired in (`Blender/Enemies/*.glb` + starter scenes on shared `enemy.gd`; static / unrigged / untextured, placeholder scale + rough sphere collision). No behaviour yet — design each (turtle = heavy/tanky "alfa predator", shell armour?; fly = likely home for the flying/moth enemy; worm = burrower / segmented crawler), tune scale/collision/orientation, wire into waves.
    - <span class="pill todo">Todo</span> Verify materials/colours imported on the 3 new models (blends had flat colours, no textures) — add per-mesh `mesh_tints` if they arrive grey. Re-export via `export_creature.py`.
    - <span class="pill todo">Todo</span> Per-element resistance/weakness system (quick-test version exists).
- <span class="pill todo">Todo</span> **Flying enemy (moth)**: parked on the anti-air question; answer agreed: aimed-throw = skill-shot AA + an AA tower upgrade. Build after aimed-throw feel is locked. *(Related: the **thief flyer** idea — steals gold/artifacts and flees — is an [open question in Decisions](decisions.md).)*
- <span class="pill wip">WIP</span> **Enemy component system**: built (`EnemyLocomotion` + `EnemyAttack` + `EnemyAbility[]`), opt-in inside `enemy.gd`. TODO: A/B `Spider_v2` vs `Spider`, port Snail, migrate the 5 subclasses, then delete the legacy inline path.
- <span class="pill todo">Todo</span> **Jumping spiders**: leapers.
- <span class="pill wip">WIP</span> **Aimed throw** (in testing): right-click lobs the selected item at a terrain point, left-click at a hovered enemy, within a 60° arc of the cart's facing (green/red reticle); SPACE drop kept. Prototype in `cart.gd`. TODO: enemy highlight for left-click, tune arc/lob feel. *(See also: "Remove the debug aim line" in High priority.)*

## Elemental interactions / combos

One element sets up (a coat/status), another pays off (a trigger).

- <span class="pill done">Done</span> **Oil + Fire = ignite** (fire spreads a burn-wave on the oil slick).
- <span class="pill done">Done</span> **Frost + Rock = brittle** — SCOPED (2026-07-17): frozen amplifies the shatter types only — PURE (rock + fall damage) and ELECTRIC, 2× (Deep Freeze 4×). Fire/poison no longer boosted; tooltips match. The frost→rock oneshot combo the HP-cap design leans on still works.
- <span class="pill done">Done</span> **Oiled enemy + Fire = instant torch** (bigger burst + fiercer burn).
- <span class="pill todo">Todo</span> **Electric + Oil/wet = arc conductor**: chain lightning jumps further to oiled neighbors.
- <span class="pill idea">Idea</span> Frost + Fire = thermal shock; Oil + Frost = ice slick; Slime/Poison + Fire = toxic flare; Frozen + heavy hit = shatter shrapnel; Lure = the universal setup.
- Pick-first recommendation: the two nearly-free ones, Oiled+Fire (done) and Electric+Oil.

## Generator & difficulty (decisions locked 2026-06-25)

- **Spider HP cap = 15**: a maxed rock can't one-shot a late spider, but frost (2× dmg) brings it back into one-shot range. Frost = the setup tool.
- **Breakable pure-shield = yes** (rock chips it, elements bypass), introduced ~wave 15. This is "rock immunity" in breakable form.
- **Powerups bought from budget**: elemental immunity (~wave 12) + pure-shield (~wave 15), ramping.
- <span class="pill todo">Todo</span> **New mechanic**: "only THIS element penetrates" layer (inverse of immunity). Visual not figured out.
- <span class="pill idea">Idea</span> Mix-2-specializations wizard tier (combine two elements).

## High priority — from playtest (2026-07-17, Cap7n)

- <span class="pill parked,">Parked, lower prio</span> **Night lighting: tower top + wizard lights** — an interim light is placed (2026-07-19); the final version waits on the lantern/flag-post 3D prop. Tracked in the [3D backlog](backlog-3d.md) (Lighting & atmosphere).
- <span class="pill done">Done</span> **Remove the debug aim line** (aimed-throw prototype leftover) from tester builds. *(Done 2026-07-18: the magenta beam + its spawn call deleted from cart.gd.)*
- <span class="pill done">Done</span> **Wave direction indicator is hidden by grass** — DONE (2026-07-19): a standalone HUD **windrose** top-centre; the arrow points at the incoming sector relative to the camera, pulses during the telegraph, steady through the assault, fades in rest. (Other ideas parked: flag rotating toward the enemy, mini-rose on the cart.)
- <span class="pill done">Done</span> **Show money invested per wizard** — DONE (2026-07-19) via the wizard shop's new **Stats screen** (Stats button by the profile cards): a panel per wizard with gold invested + every owned upgrade. Damage-done-per-wizard = backlogged under Polish.
- <span class="pill done">Done</span> **Jump from the TAB shop straight into a wizard's tree** — DONE (2026-07-19): a "Wizards" button in the TAB shop hands off to the wizard shop on the first wizard; the profile cards select from there.
- <span class="pill todo">Todo</span> **Improve the onboarding tutorial** (see the Tutorial overhaul items).

## Feedback (2026-07-19, fellow game dev)

Three points, really one arc: new players spend their attention decoding systems instead of fighting the horde. **→ Grew into the [⚡ Revamp](revamp.md) plan (temporary tab): tutorial slows down as its own mode, the main game speeds up with VS-style in-your-face choices.** The bullets below are superseded by that page.

- <span class="pill todo">Todo</span> **Separate the tutorial from the main game** — make Tutorial its own MODE (start-menu entry, scripted safe pacing, walks one system at a time). The main game starts clean. Cheap first step: `FORCE_EACH_RUN = false` + a menu "Tutorial" button that launches the level with the guided intro forced on. (Supersedes the "every run is a tutorial" playtest setting once testing settles.)
- <span class="pill todo">Todo</span> **Systems overload: players think about menus more than enemies** — design theme, not one fix. Levers: progressive unlocking (fewer systems visible in the first waves — the spellbook already hides unearned spells, artifacts already arrive at wave 1's end; consider delaying the TAB tree / artifact bag a wave or two), consolidation (fewer separate shop surfaces), and defaults that let a player IGNORE a system and still survive early waves. Pairs with the [pillars](../pillars.md) "base-game fun first" rule.
- <span class="pill todo">Todo</span> **Telegraph that upgrading exists** — players sit on gold without realizing they can/should spend it. Quick wins: a rest-phase nudge when gold is high ("Spend your gold: TAB for tower, W for wizards"), affordable-upgrade glow on the shop entry points, tutorial beat pointing at the first shop visit.

## From playtest (2026-07-17, Jen, v0.016 build)

5 waves / ~9 min / clean log. Loved the brick armour: *"i love how the tower bricks fall off btw! and you see colors for how damaged it is."* Discovered artifacts + ultimates on her own.

- <span class="pill done">Done</span> **BUG: Repair mode blocked ALL other clicking** (couldn't buy wizards / use the debug panel) — FIXED (2026-07-17): repair only consumes clicks that hit a damaged brick; clicks over any UI pass through; the highlight no longer glows behind panels.
- <span class="pill done">Done</span> **BUG: lure(?) dropped a clump of climbers who sat "stuck but alive" at the tower base** — FIXED via a full redesign (2026-07-18, Cap7n's design call): **the lure is now a PLAIN dropped item with the lure AoE attached** — no special bait body at all. `LureBeacon` DELETED; `LureEffect` runs the pull / one-bite-per-enemy / edible-HP decay directly on the standard lingering item. Standard physics + standard throw arc (the interim RigidBody beacon's `linear_damp` shortened throws: same lob 12.9 m damped vs 23.5 m as a normal item; the original Node3D raycast version buried the orb ~3 m under the field = the stuck clump). New reusable pipeline hook: `DroppedItem.linger_hold` pauses the 6 s linger timer for effects that own their lifetime. Visual: `Items/Scenes/lure_orb.tscn` (the beacon's emissive pink sphere, editor-tunable) replaced the pink cube for world + held; the OmniLight still attaches in code on drop (held copies stay light-free) and is now CENTRED in the orb — its old 0.6 m offset swung around the tumbling item and buried the glow in the floor ("light disconnects on landing").
- <span class="pill done">Done</span> **W far from a goblin did nothing** — FIXED (2026-07-17): flashes "No wizard in reach — ride the cart under one and press W, or just click a wizard."
- <span class="pill done">Done</span> **"I misunderstood repair wall"** — FIXED (2026-07-17): button reads "Repair Wall (click bricks)" / "Repairing: click bricks" + tooltip and status line state the 1g/HP rule.

## From playtest (2026-07-17, Jennifer's sister, v0.016 build)

26 min / 12 waves / zero errors / 20 in-game notes. Economy data cheat-tainted (used the F4 gold — BalanceLogger now flags that).

- <span class="pill done">Done</span> **"Double tower coats causes some lag"** — first-use shader-compile stutter addressed with the **ShaderPrewarm autoload** (2026-07-17: every VFX shader + instanced variant + particle sim renders sub-pixel during the grace phase). If lag persists it's the sustained cost of two coats at once → profile FluidField.
- <span class="pill done">Done</span> **Hold-to-pour should stop on release** — DONE (2026-07-17): releasing SPACE mid-pour ends the stream; the rest stays in the bin. Tap = one, hold = pour-while-held.
- <span class="pill done">Done</span> **Click-outside-to-close menus** — DONE (2026-07-19), scoped by Cap7n to the ESC/pause menu + the F4 cheat panel (not the wizard trees): clicking the world outside either closes it.
- <span class="pill done">Done</span> **F4 debug panel overlaps the inventory UI** — DONE (2026-07-19): moved right of the inventory column and raised.
- <span class="pill todo">Todo</span> **"Can I save my progress?"** — decide: mid-run save, or an in-game hint that runs are one-shot.
- **Idea pile** (curate): water element · wizard idle/flavor animations · intro/milestone cutscenes · wizard-slot counts as difficulty (hard = 3 slots) · ultimates pricier as the run progresses · biomes/levels · skins for wizards/mobs/cart · chicken mob · **ranged mobs that shoot up at the tower** · achievements that unlock things · surprise chests · flag leagues/clans · **music intensifies as waves ramp** · cherry blossom trees.

## From playtest (2026-07-12, Cap7n)

- <span class="pill done">Done</span> **Tutorial: force ROCK as the first wizard pick** — DONE, then REWORKED (2026-07-17) into a standalone per-run rule: non-Rock elements lock until the run's FIRST tree purchase (any rock node; Bigger Rock lvl 1 costs 1g so the first shop visit always affords it), unlocking live the moment it's bought. Decoupled from tutorial beats entirely (`TutorialDirector.rock_gate_active`); locked buttons say "(buy a Rock upgrade first)".
- <span class="pill done">Done</span> **Wave-skip (Enter) → wizards instantly finish their current conjure** — DONE (2026-07-12): every wizard snaps its conjure on `wave_started` (any wave start, not just skips) and delivers via the normal flow.
- <span class="pill done">Done</span> **BUG: grass trample stops working on wave 2+** — fixed TWICE: (1) coverage — the map only covered ±90 m vs the 110–150 m spawn band, now ±160 m @ 768 px (2026-07-12); (2) cache eviction — the wired grass material was FREED while the (grass-free) start menu was up, so any menu→Play run had trample dead from wave 1; the autoload now holds a strong ref + re-wires on level entry (2026-07-16). Editor F5-the-level runs masked bug 2 by keeping the cache alive.
- <span class="pill done">Done</span> **REPAIR system (hammer cursor)** — DONE (2026-07-16), upgraded beyond the spec: the brick shell became a two-layer HP system (armour that soaks before the core bar), click-to-repair with per-brick static colliders, 1g/HP, hammer cursor that swings on click. Still open from the spec: repair amount/speed upgrades in the TAB shop + a pricier full-rebuild surcharge.

## From playtest C4 (wave 39+ marathon)

- <span class="pill wip">WIP</span> **Kill the snowball**: went unkillable past wave 10 (spikes insta-kill + regen heals all back). Done: removed spikes / poisoned spikes / HP regen from the TAB tree (systems kept). Next: budget wave generator, late-game tuning.
- <span class="pill todo">Todo</span> **Spikes → Rock-wizard ULTIMATE** (decided 2026-07-12: cast-for-gold with a duration, like Oil/Frost/Slime — not a permanent upgrade). Folds into the ultimates-gating pass; `base_spikes`/`poisoned_spikes` tree nodes + the SpikeRing mechanics (still in shop_graph) are the ingredients.
- <span class="pill todo">Todo</span> **HP regen → upgradeable spell**, not a forever passive. *(Or superseded by the new REPAIR-hammer idea, see 2026-07-12 playtest notes.)*
- <span class="pill todo">Todo</span> **Frost is underpowered**: concept: snow-storm on hit (slow + DoT AoE), "avalanche" upgrade.
- <span class="pill todo">Todo</span> **Fire & lightning too cheap vs rock**: element cost rebalance.
- <span class="pill todo">Todo</span> **BUG: lightning's spread fires on ROCK kills**: element effect bleeding across equipped wizards.
- <span class="pill todo">Todo</span> **Roller (pillbug) aiming is un-fun**: make the path visible or the orbit predictable.

## Shop / trees: Pass 2 wiring

- <span class="pill done">Done</span> **Rename-reconnect pass (2026-07-12):** 13 tree effects that silently did nothing (the tree rebuild renamed node ids; the shop kept listening for the old names) are re-pointed and live again — frost Deep Freeze / Let It Snow / Sandals, fire More-DoT / Lingering Flame, poison Muscle Degradation / Spreading Death, all four lure poison/paralyze/wider-pull nodes. Frost + poison + lure went from mostly-placebo to functional.
- <span class="pill todo">Todo</span> **~20 nodes still need NEW effect code** (never built): wet_conductor, wildfire, ricochet, boulder, sharp_rocks_add_bleed, sharper_spiky_bits, shatter, chill_to_the_bone, endless_winter, frost_slow_target, black_lung, crude_oil, patient_zero, thicker_mucus, overload, more_starts, pulse_fire, quiker_pulses, lure_pull_horde_to_spot, black_hole_lure. Element-by-element sessions.
- <span class="pill wip">WIP</span> **Ultimates gating + the BOOK spellbook** — **v1 BUILT (2026-07-19)**: the spellbook is now an actual two-page book (placeholder parchment, code-built) with element ribbon tabs, opened instantly from the right-edge pull tab, non-modal (no dim). **Gating LIVE**: a spell casts only once a wizard of that element owns its `ultimate_*` tree node; unowned spells are **fully hidden** (Cap7n call: listing everything up front overwhelms), empty pages read "Nothing scribed yet. Ultimates learned in the wizard trees appear here."; owned-but-unbuilt ones say "(spell not built yet)". Castable now: Oil, Deep Freeze, Slime, **Meteor Shower** (new `ultimate_meteors` node added to the Rock tree, 60g, off Bigger Rock). Still open: FireWave / Arc Storm / Black-hole Lure effect code, spikes folded in as a Rock spell, granting-wizard buff inheritance, per-spell costs (flat 15g placeholder), artist book art + page-flip, and syncing `ultimate_meteors` into the skill-tree-maker tool.
- <span class="pill done">Done</span> Bleed visual: code-built red drip particle toggled by the `bleed` status.
- <span class="pill todo">Todo</span> Per-level wizard costs: wire `costs[]` into `wizard_shop_ui`.
- <span class="pill todo">Todo</span> Magnitude tuning to match the new tree descriptions.
- <span class="pill todo">Todo</span> **Elemental spiders**: fire spiders ~W3 (immune to fire + prompt), elemental types trickling in from ~W5.

## Enemy polish (parked)

- **Spider:** [ ] attack animation (dug-in spiders currently freeze mid-walk-cycle while gnawing).
- **Snail:** [ ] UV + texture; [ ] fracture shell into shards (progressive cracking = the health bar); [ ] squish-death; [ ] skirt-ripple shader; [ ] tune collision shape.
- **GroundStain system**: [ ] **(idea)** generalize `FluidField`'s stain layer onto flat terrain for snail slime trails (later oil/blood/scorch). Start CPU stain-only on a coarse grid; move to a GPU render-target if scale demands.
- **Frost:** [ ] icicles on the rim; [ ] steam/mist (`FrostFx`); [ ] true ice nucleation; [ ] tuning.
- **Slime:** [ ] reuse the parked `tendrils()` drip; [ ] look tuning.

## Artifacts (scaffold)

- <span class="pill wip">WIP</span> Relics dropped from kills → small passive bonuses (right-edge tab/panel). Loop works (drop → grant → bonus); gold_per_kill + tower_max_hp live. TODO: real catalogue, wire queried bonuses (cart_speed / rock_damage / conjure), decide meta-progression persistence.
- <span class="pill wip">WIP</span> **PLACEABLE artifacts on the tower wall** — v1 BUILT (2026-07-19): placement mode (panel button → wall-snapped ghost → click to mount, right-click/Esc cancels), effects run ONLY while placed and ONLY during live waves; **Coin Maker** granted at end of wave 1 (a coin arcs to the pile every 5 s; magenta 0.3×3×3 slab placeholder). The seed of the **traps / hang-from-the-tower family**. **Pass 2 (same day):** cube fixed to 0.3³; per-artifact **`scene` key** in the catalogue (each artifact can bring its own mesh scene, root script extends `TowerArtifact`, `mount_offset` export seats it on the surface); two placement checks per artifact: **`hang`** (side wall) and/or **`top`** (tower-top floor / rim / side bits, upward faces on the upper half). **Pass 3 (same day):** ALL 6 artifacts placeable — the 5 old bag passives now only work while MOUNTED (`bonus()` counts placed copies; Thick Bark's +max HP applies on mount), the coin tick is Coin Maker only, the cube is 0.5³ and tinted per artifact colour. **Pass 4/5 (same day):** the ✦ panel is a 3×3 icon grid (click icon = place, hover = tooltip, badge = bag copies; placed copies LEAVE the bag); all 3 queried bonuses wired into their consumers (cart speed, conjure time with a 25% floor, rock damage) — stacking decided: **additive from base** (two +5% = +10%, not compounding). **Pass 6 (same day):** the **Mover tool** (button beside the Repair tray, debug looks): click a mounted artifact to pick it up and re-place it, a quick second click banks it to the bag (on-place bonuses revert correctly); plus **continuous placement** (keep placing copies until the bag runs dry or right-click/Esc). **Pass 7 (same day):** **brick-link knock-off** — a wall-hung artifact links to the brick it hangs on (same coverage rule as armor damage); if that brick BREAKS the artifact is knocked loose and returns to the bag (toast), top mounts stay safe. Wall = close to the action but destructible, top = safe: placement is now a real decision. *(Upgrade idea, parked: knocked artifacts physically DROP as a pickup instead of teleporting to the bag.)* **Pass 8 (same day): the first 3 TURRET artifacts** (each its own scene via the catalogue `scene` key): **Laser Lens** (beams + slows the nearest enemy in range), **Dart Spitter** (poison dart at the nearest enemy every 3 s), **Oil Dripper** (pours the tower oil coat for 6 s every 25 s; v1 = whole-tower, becomes a local drip when FluidField grows a localized pour; plays rough with the Oil ultimate for now). The **water dripper** variant (electric conductor combo) is DEFERRED: no water fluid exists in FluidField yet — that is a system to design first. Catalogue = 9 artifacts, exactly filling the 3×3 bag. Next: real models, a pretty mover cursor, placement slots/limits, localized fluid pour, the water fluid. NOTE: a green-zone spot only accepts placement if that mesh has collision (parapet bits without colliders pass the ray through).

- <span class="pill todo">Todo</span> **Unique beam VFX per element** — all 6 `element:beam` artifacts currently share one pack scene (`laser_vfx_01`), differing only by recolor, so a fire beam and a frost beam look identical apart from hue. Give each element its own beam identity: pick a distinct Binbun `laser_vfx_0X` / `beam_vfx_0X` preset per element (chunky vs thin vs jagged), and/or per-element impact behaviour (fire beam ignites, frost beam hard-slows, electric arcs). One-line preload swap per element; wire via the catalogue so each `*_beam` entry names its own scene.

## Art / style

- <span class="pill todo">Todo</span> **Stylized art pass**: settle a consistent look once real models exist. Parked toon-shader tech in `VFX/Shaders/toon*.gdshader`. No outlines (faceted / soft-matte).

## Polish / quick wins

- <span class="pill todo">Todo</span> **VFX for aim line** — the cannon-shot trajectory line (hold right-click) is currently a plain translucent ribbon that reads like a debug draw. Give it a proper magical treatment: shader with flow/scroll along the path, soft glow, maybe dashes or sparkle drift; keep the purple = clear / red = blocked language. Lives in `cart.gd` (`_make_aim_line` / `_draw_aim_line`, camera-facing ImmediateMesh ribbon, `aim_line_width` export).
- <span class="pill todo">Todo</span> **Per-wizard damage tracking** — record damage dealt per wizard (attribute each summon's hits to its wizard) and show it on the shop's Stats screen. The Stats screen itself (gold invested + owned upgrades per wizard) shipped 2026-07-19.
- <span class="pill todo">Todo</span> **Goblin wizard: more animations** — a real Idle (a procedural one already ships unused inside `Goblin.glb`), hat-tip on purchase, ear wiggle (ear bones exist), a "ta-dah" when an item finishes. The archer model in the same folder can reuse the whole rig pipeline.
- <span class="pill todo">Todo</span> **Grass trample: smooth the pop-back** — blades still rise a touch fast right behind a walker (the just-left spot only ever got the stamp's soft skirt). Ideas: max-blend stamping, ease the shader response (str²), widen the flat core.
- <span class="pill done">Done</span> Tower HP bar shrink / reposition.
- <span class="pill done">Done</span> Flame VFX: visible ignite/burning on enemies.
- <span class="pill done">Done</span> Cart inventory cap → 9.
- <span class="pill done">Done</span> item_bench: REPLACED (2026-07-21) by `vfx_bench.tscn`, the unified test bench (VFX triggers tab + real-path item drops tab, dummy spider row).
- <span class="pill todo">Todo</span> Tuning pass on the 6 element trees + TAB base tree + lure feel.
- <span class="pill todo">Todo</span> Day/night cadence (currently 240s), refine later.

## Known bug watch

- <span class="pill wip">WIP</span> Restart-run crash (build-only, doesn't repro in editor). Crash logging added (`user://logs/godot.log` bundled into `balance_logs/engine_logs/`); waiting on a captured crash log.
- <span class="pill todo">Todo</span> Game file size + lag pass (overlaps Perf Phase 0).

## Ship prep (before any public launch)

- <span class="pill done">Done</span> **RBL: Roll-Call assets REMOVED (2026-07-19)** — `Imported/RollCall/` (102 MB: PureNature rocks/grass, 56 JMO CartoonFX textures, skyboxes) deleted from the working tree. Every dependent was rewired to OWNED procedural replacements first: 6 generated low-poly rocks (`Items/Meshes/Rocks/own_rock_*.glb`, Blender-generated, ours) drive the rock + ice item visuals; the rock material is texture-free flat; the grass patch is now built from **Quaternius MegaKit clumps** (CC0, real leafy shapes; Grass.png palette texture; same `.res` path + footprint, so the baked scatter needed zero level edits — a generated crossed-quad version served as the interim). **BONUS FIND: the 6 frost-crust rocks were Sketchfab rips** (unknown licence = RBL by the house rule) — overwritten with the generated rocks, size-matched. Zero RollCall/Sketchfab references remain in sources. Jennifer's real rock ask (3D backlog) still stands as the QUALITY upgrade; the generated ones are the legal placeholder.
- <span class="pill todo">Todo</span> **Git HISTORY still contains the RBL files** — the repo must stay **private** until history is purged (`git filter-repo` on `Imported/RollCall/`) or accept-and-never-open-source. Do the purge before any public/source release.
- <span class="pill todo">Todo</span> Cosmetic leftover: `Grass_RollCall.res/.tscn` file NAMES kept (contents are ours now) because level_1/menu_world reference the paths — rename in a calm editor session someday.

---

## Recently done

- **2026-07-19 (v0.017, part 2 — the ARTIFACT sprint):** artifacts went scaffold → **full physical system in six passes, zero bugs**: placeable `TowerArtifact` relics (0.5³ tinted cubes, per-artifact `scene` key for real models later) mounted via ghost placement on **two surface types** (hang = side wall, top = tower top); effects run ONLY while mounted and only during live waves; **all 6 artifacts live** (Coin Maker granted end of wave 1 + the 5 ex-bag passives wired into cart speed / conjure floor 25% / rock damage / gold per kill / tower max HP with on-mount apply + on-pickup revert); stacking decided **additive from base**; the ✦ bag is a **3×3 icon grid** (click = place, hover = tooltip, placed copies leave the bag, continuous placement until dry); **Mover tool** beside the Repair tray (pick up / re-place / double-click banks). New plumbing: `wave_completed` signal emitted, `spawn_bonus_coin`, collision layer 8 ArtifactPick.
- **2026-07-19 (v0.017):** **SPELLBOOK v1** — an actual two-page book (placeholder parchment, element ribbon tabs, non-modal fast open) with **tree-gated spells**: buying a wizard's `ultimate_*` node scribes it into the book; unowned spells hidden. **Meteor Shower** built (new Rock ultimate node; rains real rock items over the incoming sector) and rocks now **CRUMBLE** on ground impact (`fx_crumble`, reusable no-glow debris burst) instead of vanishing. Cleared Cap7n's high-prio list: wizard **Stats screen** (gold invested + owned upgrades per wizard), **Wizards button** in the TAB shop, **windrose** wave-direction compass (bottom right, perspective-flattened; 3D prop = looks upgrade later). Sister's QoL: F4 panel moved off the inventory, click-outside closes pause menu + F4 panel. **Conjure timers** settings toggle (off by default, element-coloured countdowns). **Cart v2 model** swapped in (game + menu, wheel-centred to keep the rail fit; original blend untouched). New coin mesh (sparkles off pending a better shader fix); F3 gold-pour film cheat (cheat-flagged). Debug aim beam + drop line removed. Night lighting parked on the lantern prop (3D backlog). Win condition + Mana logged as open questions; spellbook-cast bonus inheritance decided (not built). CHANGELOG v0.017 written.
- **2026-07-18 (post-v0.016 playtest round):** **Auto next wave** — a persistent "Auto next" checkbox above the "Press Enter" prompt (always on screen, toggleable mid-wave; ON = the 60 s rest collapses to a 5 s countdown, `wave_manager.auto_next` + `auto_next_rest_time`, saved to `user://gameplay.cfg`; auto-started waves snap in-progress conjures exactly like the Enter skip — same `wave_started` signal, verified). **Element switching redesigned** — the bottom 6-card switch row is gone; the tree's BASE node is a gold "Change element" doorway back to the picker (respec/refund still only on picking a DIFFERENT element there; reopening lands on the picker until a tree is entered). **Wizard profile cards** — numbered element-tinted cards at the shop's bottom replace the Prev/Next edge arrows; click = jump to that wizard, current one lit; ordered by rim slot / hire order (the name-sort scrambled them). **Lure redesign** — plain item + AoE (see the Jen playtest entry above), glowing orb visual, centred light. Backlog consolidated INTO this wiki page (old artifacts-folder file deleted; 3D + 2D/UI sibling scaffolds added).
- **2026-07-16/17 (v0.016):** Two-layer tower HP — the brick shell is local ARMOUR (10 HP/brick, coverage radius) soaking hits before the HP bar, bricks tint/crack/tumble off; **Repair tool** (R / button, click bricks, 1g/HP, hammer cursor); **Settings menu** (volumes/fullscreen/resolution); Rock-first element gate as a standalone purchase-triggered rule; Frozen scoped to PURE+ELECTRIC; Esc closes the TAB shop; wizards face outward; grass-trample cache-eviction fix; environment polish (procedural ground + dirt apron, tower dirt skirt, tiki torches as a placeable scene + menu ring, radial wood-tie shader); game icon (native exe embedding); **ShaderPrewarm** (kills first-cast hitch); BalanceLogger fps/worst-frame + cheat-taint columns; pour stops on release; repair-mode click-through + self-explaining labels; W-out-of-reach hint. v0.016 shipped to testers; two tester logs analyzed same-day.
- **2026-07-12:** Goblin wizard LIVE in-game (rigged + Conjure loop + per-element outfit recolor); 13 orphaned skill-tree effects reconnected (frost/poison/lure work again); artifact toast moved under the HP bar (10s); spikes redefined as a Rock ULTIMATE.
- **Living grass**: PureNature grass swap (dense, tinted, shadow-free) + wind sway shader + trample map — enemies part the grass, 60s spring-back trails.
- Game renamed → **"Tower Drop"**; menu patch-notes panel (fed from `CHANGELOG.txt`).
- Spellbook ultimates (Oil/Frost/Slime, 15g/10s) that actually affect enemies.
- Fire pass: stylized flame, ground-impact ignite + enemy-to-enemy spread, Oiled+Fire torch.
- DEBUG SANDBOX panel (F4) replacing scattered debug keys.
- Wizard shop: Prev/Next wizard arrows, hover tooltips.
- New modeler tower imported + brick-shell MultiMesh with runtime damage states.
