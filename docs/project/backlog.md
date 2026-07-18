# Backlog

A living to-do list. Edit freely: tick boxes, add items, reorder.

**Status tags:** `[x]` done · `[ ]` todo · **(WIP)** in progress · **(idea)** undecided · **(check)** may already be done.

---

## Playtest build v0.1

Goal: a stable, understandable build that's beatable to ~wave 10.

### Tier 0: build hygiene

- [x] Bake the ProtonScatter foliage in `level_1` and `menu_world` (kills load-time pop-in); set `force_rebuild_on_load = false`.
- [x] Disable the snail wave-1 test hook (wave 1 = spiders via curated list; pillbugs W6, snails W8).
- [ ] **(WIP)** Cheat keys → **DEBUG SANDBOX panel** (F4 opens a button panel instead of dumping gold). `DEBUG_TOOLS` kept ON for testers deliberately. Fold remaining raw keys into the panel before a wider build.
- [x] Kill debug overlays + `Globals.DEBUG` console spam (only the BalanceLogger per-wave line prints).
- [ ] Exclude test scenes (`item_bench`, `cart_test`) from the player flow.
- [x] Visible version/build label on the menu ("Tower Drop" + version string). Bump per build.

### Tier 1: core loop (must work)

- [ ] **(WIP)** Shops:
    - [ ] **(WIP)** Wire the 3 tower **ultimates**, castable now via the right-edge Spellbook. TODO: decide if the final home is the spellbook or per-element wizard-tree abilities.
    - [ ] **Conjuring-time UI**: show how long the goblin shop takes to conjure (progress ring / countdown).
    - [x] Per-level costs wired (`wizard_shop_ui._node_cost` uses the `costs[]` array). Rebalance still open.
    - [ ] **(WIP)** Tooltips / descriptions: hover tooltips wired; copy still needs filling in + spellcheck.
- [ ] **Waves beatable to ≥10**: tune size/HP ramp and make sure defenses keep up.
- [ ] **Item→effect coverage**: every buyable/usable item must actually do something (no dead items).

### Tier 2: onboarding

- [ ] **Tutorial overhaul**: walk the actual systems (cart drop/pour + aimed throw, element trees, base + wizard shops, build/traps, tower ultimates), not just intro hints.
- [ ] **Controls reference** card (move / drop / throw / shop keys), a pause-screen list at minimum.

### Tier 3: game shell (mostly exists, verify + fill gaps)

- [x] Start menu, pause menu, game-over, export preset exist → verify end-to-end.
- [x] Title screen rebuilt: live 3D backdrop (cart orbits tower) + button column + day/night + fog. Settings/Compendium/Profiles buttons are stubs.
- [ ] Menu fog still looks a bit weird: revisit `menu_atmosphere` fog tuning.
- [x] Bake the menu scatter (same as level_1) so the menu loads instant.
- [ ] **Win / summary state** at wave 10 (survived N waves); only game-over exists now.
- [ ] Clean **restart** after game-over without relaunching.
- [x] **Settings** — DONE (2026-07-16): music/sound volume (real audio buses), fullscreen, windowed resolution; persists to `user://settings.cfg`; reachable from the start AND pause menus.
- [ ] **(WIP)** Tester feedback channel: in-game note (press C) exists; still want a Discord/report link visible.

### Tier 4: looks-finished polish

- [x] **Wizards replaced (2026-07-12):** the rigged goblin wizard (`Blender/Goblin.glb`) stands in for every shop wizard — Conjure anim looping 24/7 with a 30° gaze-up, the shared `purple` cloth material recolors the whole outfit per element (darkened when specialized), power tier = hat size. More anims (real Idle, hat-tip, ear wiggle) = polish backlog. *(The item cubes for fire/electric/frost/lure items are separate and still placeholder.)*
- [x] Confirm hit feedback is clear without damage numbers: body hit-flash only.
- [ ] Tower HP bar shrink / reposition; HUD readability pass (wave / HP / gold / loadout).
- [ ] **(WIP 2026-07-16)** Audio: key SFX + music balance. `Sfx` autoload built (UI = flat/centered, world = positional 3D, pitch wobble + throttle, auto-click on every button). Wired: clicks, coins, skill unlock, spider bite, pillbug slam, wall repair (Kenney CC0, `Game Sounds/UsedSounds/`). Still missing: ultimate casts, wave-warning tie-in, item impacts, enemy deaths. *(Volume settings: DONE via the settings menu, 2026-07-16.)*
- [ ] **Perf Phase 0 quick wins** (shadows, alloc flood, ragdoll caps) so wave 10 doesn't chug.

---

## Tower ultimates

- [x] **Rock: Oil the tower** (oil coat + fire-wave). Key `O` / `L` ignite.
- [x] **Frost: Freeze the tower** (ice rocks + ice wall coat). Key `I`.
- [x] **Poison: Slime the tower** (jiggly mucus, slow+poison). Key `P`.
- [ ] **Rock ultimate: Meteorite Shower** — a rock-wizard ultimate that rains meteorites down on the enemies (AoE bombardment from above). New idea, distinct from the existing "Oil the tower" rock coat.
- [ ] **(idea)** Do Fire / Electric / Lure want their own tower ultimates too? (6-element set)
- [ ] Tower ultimates gated **deep in the wizard tree** rather than the always-on spellbook.

## Bigger features (later)

- [ ] **(WIP)** **Tower-damage rework**: spiders dig in & attack at a random wall height. Pass 1 done (band 40→95% of top, `attack_height_min/max_frac`). TODO: HP/damage rebalance.
- [x] **Visible tower damage** — DONE (2026-07-16) via the brick-ARMOUR rework: the 110-brick shell is a real armour layer (local coverage radius) that soaks attacks BEFORE the HP bar; bricks tint, crack and tumble off (physical debris) where enemies dig in; the repair tool rebuilds them. See [The Tower](../game/tower.md).
- [ ] **Buildable defenses**: satellite towers + traps (TAB-shop → build tray → click-to-place). Iter-1 exists but free / no purchase.
- [ ] **Traps**: more placeable ground hazards (caltrops / grinder / conveyor / chain); new plan: buy at a wizard then throw into place (reuse aimed-throw).
- [ ] **(WIP)** **More enemy types**: scripts done (thin `extends Enemy` subclasses); each needs a model + scene:
    - [x] **Pillbug**: done + feels good (curl-morph roll, charge-bonk-stun ability). Polish: real leg-wiggle shape key, UV texture, wire into waves. Redesign: replace slow float-drop with a thrown "shot".
    - [ ] **Termite**: fast ~1HP swarm, chews wall fast.
    - [ ] **Bombardier**: death-cloud corrodes the tower; resists poison.
    - [ ] **Carrier**: always hauls an item, tankier.
    - [ ] **Centipede**: head tows follower segments; first pass exists. TODO: per-segment colliders, mid-split, status propagation.
    - [x] **Snail**: in-game (shell-armour gates the body). Polish parked below.
    - [ ] **(WIP)** **Turtle / Fly / Worm** (modeler's 3 creatures): models wired in (`Blender/Enemies/*.glb` + starter scenes on shared `enemy.gd`; static / unrigged / untextured, placeholder scale + rough sphere collision). No behaviour yet — design each (turtle = heavy/tanky "alfa predator", shell armour?; fly = likely home for the flying/moth enemy; worm = burrower / segmented crawler), tune scale/collision/orientation, wire into waves.
    - [ ] Verify materials/colours imported on the 3 new models (blends had flat colours, no textures) — add per-mesh `mesh_tints` if they arrive grey. Re-export via `export_creature.py`.
    - [ ] Per-element resistance/weakness system (quick-test version exists).
- [ ] **Flying enemy (moth)**: parked on the anti-air question; answer agreed: aimed-throw = skill-shot AA + an AA tower upgrade. Build after aimed-throw feel is locked.
- [ ] **(WIP)** **Enemy component system**: built (`EnemyLocomotion` + `EnemyAttack` + `EnemyAbility[]`), opt-in inside `enemy.gd`. TODO: A/B `Spider_v2` vs `Spider`, port Snail, migrate the 5 subclasses, then delete the legacy inline path.
- [ ] **Jumping spiders**: leapers.
- [ ] **(WIP)** **Aimed throw** (in testing): right-click lobs the selected item at a terrain point, left-click at a hovered enemy, within a 60° arc of the cart's facing (green/red reticle); SPACE drop kept. Prototype in `cart.gd`. TODO: enemy highlight for left-click, tune arc/lob feel. *(See also: "Remove the debug aim line" in High priority.)*

## Elemental interactions / combos

One element sets up (a coat/status), another pays off (a trigger).

- [x] **Oil + Fire = ignite** (fire spreads a burn-wave on the oil slick).
- [x] **Frost + Rock = brittle** — SCOPED (2026-07-17): frozen amplifies the shatter types only — PURE (rock + fall damage) and ELECTRIC, 2× (Deep Freeze 4×). Fire/poison no longer boosted; tooltips match. The frost→rock oneshot combo the HP-cap design leans on still works.
- [x] **Oiled enemy + Fire = instant torch** (bigger burst + fiercer burn).
- [ ] **Electric + Oil/wet = arc conductor**: chain lightning jumps further to oiled neighbors.
- [ ] **(idea)** Frost + Fire = thermal shock; Oil + Frost = ice slick; Slime/Poison + Fire = toxic flare; Frozen + heavy hit = shatter shrapnel; Lure = the universal setup.
- Pick-first recommendation: the two nearly-free ones, Oiled+Fire (done) and Electric+Oil.

## Generator & difficulty (decisions locked 2026-06-25)

- **Spider HP cap = 15**: a maxed rock can't one-shot a late spider, but frost (2× dmg) brings it back into one-shot range. Frost = the setup tool.
- **Breakable pure-shield = yes** (rock chips it, elements bypass), introduced ~wave 15. This is "rock immunity" in breakable form.
- **Powerups bought from budget**: elemental immunity (~wave 12) + pure-shield (~wave 15), ramping.
- [ ] **New mechanic**: "only THIS element penetrates" layer (inverse of immunity). Visual not figured out.
- [ ] **(idea)** Mix-2-specializations wizard tier (combine two elements).

## High priority — from playtest (2026-07-17, Cap7n)

- [ ] **Light the top of the tower at night** — the rim/top goes dark during the night phase; add a light up there (torch / rim glow / spotlight).
- [ ] **Lights next to each wizard** — so they read at night and the shop is findable in the dark.
- [ ] **Remove the debug aim line** (aimed-throw prototype leftover) from tester builds.
- [ ] **Wave direction indicator is hidden by grass** — the telegraph gets covered by foliage. Ideas: a shine/glow behind the trees in the incoming sector, a marker, or a windrose/compass arrow.
- [ ] **Show money invested per wizard** (running total on the tree screen).
- [ ] **Jump from the TAB shop straight into a wizard's tree.**
- [ ] **Improve the onboarding tutorial** (see the Tutorial overhaul items).

## From playtest (2026-07-17, Jen, v0.016 build)

5 waves / ~9 min / clean log. Loved the brick armour: *"i love how the tower bricks fall off btw! and you see colors for how damaged it is."* Discovered artifacts + ultimates on her own.

- [x] **BUG: Repair mode blocked ALL other clicking** (couldn't buy wizards / use the debug panel) — FIXED (2026-07-17): repair only consumes clicks that hit a damaged brick; clicks over any UI pass through; the highlight no longer glows behind panels.
- [x] **BUG: lure(?) dropped a clump of climbers who sat "stuck but alive" at the tower base** — FIXED via a full redesign (2026-07-18, Cap7n's design call): **the lure is now a PLAIN dropped item with the lure AoE attached** — no special bait body at all. `LureBeacon` DELETED; `LureEffect` runs the pull / one-bite-per-enemy / edible-HP decay directly on the standard lingering item. Standard physics + standard throw arc (the interim RigidBody beacon's `linear_damp` shortened throws: same lob 12.9 m damped vs 23.5 m as a normal item; the original Node3D raycast version buried the orb ~3 m under the field = the stuck clump). New reusable pipeline hook: `DroppedItem.linger_hold` pauses the 6 s linger timer for effects that own their lifetime. Visual: `Items/Scenes/lure_orb.tscn` (the beacon's emissive pink sphere, editor-tunable) replaced the pink cube for world + held; the OmniLight still attaches in code on drop (held copies stay light-free) and is now CENTRED in the orb — its old 0.6 m offset swung around the tumbling item and buried the glow in the floor ("light disconnects on landing").
- [x] **W far from a goblin did nothing** — FIXED (2026-07-17): flashes "No wizard in reach — ride the cart under one and press W, or just click a wizard."
- [x] **"I misunderstood repair wall"** — FIXED (2026-07-17): button reads "Repair Wall (click bricks)" / "Repairing: click bricks" + tooltip and status line state the 1g/HP rule.

## From playtest (2026-07-17, Jennifer's sister, v0.016 build)

26 min / 12 waves / zero errors / 20 in-game notes. Economy data cheat-tainted (used the F4 gold — BalanceLogger now flags that).

- [x] **"Double tower coats causes some lag"** — first-use shader-compile stutter addressed with the **ShaderPrewarm autoload** (2026-07-17: every VFX shader + instanced variant + particle sim renders sub-pixel during the grace phase). If lag persists it's the sustained cost of two coats at once → profile FluidField.
- [x] **Hold-to-pour should stop on release** — DONE (2026-07-17): releasing SPACE mid-pour ends the stream; the rest stays in the bin. Tap = one, hold = pour-while-held.
- [ ] **Click-outside-to-close menus** — close shops by clicking the world instead of only the toggle button.
- [ ] **F4 debug panel overlaps the inventory UI** — move the panel.
- [ ] **"Can I save my progress?"** — decide: mid-run save, or an in-game hint that runs are one-shot.
- **Idea pile** (curate): water element · wizard idle/flavor animations · intro/milestone cutscenes · wizard-slot counts as difficulty (hard = 3 slots) · ultimates pricier as the run progresses · biomes/levels · skins for wizards/mobs/cart · chicken mob · **ranged mobs that shoot up at the tower** · achievements that unlock things · surprise chests · flag leagues/clans · **music intensifies as waves ramp** · cherry blossom trees.

## From playtest (2026-07-12, Cap7n)

- [x] **Tutorial: force ROCK as the first wizard pick** — DONE, then REWORKED (2026-07-17) into a standalone per-run rule: non-Rock elements lock until the run's FIRST tree purchase (any rock node; Bigger Rock lvl 1 costs 1g so the first shop visit always affords it), unlocking live the moment it's bought. Decoupled from tutorial beats entirely (`TutorialDirector.rock_gate_active`); locked buttons say "(buy a Rock upgrade first)".
- [x] **Wave-skip (Enter) → wizards instantly finish their current conjure** — DONE (2026-07-12): every wizard snaps its conjure on `wave_started` (any wave start, not just skips) and delivers via the normal flow.
- [x] **BUG: grass trample stops working on wave 2+** — fixed TWICE: (1) coverage — the map only covered ±90 m vs the 110–150 m spawn band, now ±160 m @ 768 px (2026-07-12); (2) cache eviction — the wired grass material was FREED while the (grass-free) start menu was up, so any menu→Play run had trample dead from wave 1; the autoload now holds a strong ref + re-wires on level entry (2026-07-16). Editor F5-the-level runs masked bug 2 by keeping the cache alive.
- [x] **REPAIR system (hammer cursor)** — DONE (2026-07-16), upgraded beyond the spec: the brick shell became a two-layer HP system (armour that soaks before the core bar), click-to-repair with per-brick static colliders, 1g/HP, hammer cursor that swings on click. Still open from the spec: repair amount/speed upgrades in the TAB shop + a pricier full-rebuild surcharge.

## From playtest C4 (wave 39+ marathon)

- [ ] **(WIP)** **Kill the snowball**: went unkillable past wave 10 (spikes insta-kill + regen heals all back). Done: removed spikes / poisoned spikes / HP regen from the TAB tree (systems kept). Next: budget wave generator, late-game tuning.
- [ ] **Spikes → Rock-wizard ULTIMATE** (decided 2026-07-12: cast-for-gold with a duration, like Oil/Frost/Slime — not a permanent upgrade). Folds into the ultimates-gating pass; `base_spikes`/`poisoned_spikes` tree nodes + the SpikeRing mechanics (still in shop_graph) are the ingredients.
- [ ] **HP regen → upgradeable spell**, not a forever passive. *(Or superseded by the new REPAIR-hammer idea, see 2026-07-12 playtest notes.)*
- [ ] **Frost is underpowered**: concept: snow-storm on hit (slow + DoT AoE), "avalanche" upgrade.
- [ ] **Fire & lightning too cheap vs rock**: element cost rebalance.
- [ ] **BUG: lightning's spread fires on ROCK kills**: element effect bleeding across equipped wizards.
- [ ] **Roller (pillbug) aiming is un-fun**: make the path visible or the orbit predictable.

## Shop / trees: Pass 2 wiring

- [x] **Rename-reconnect pass (2026-07-12):** 13 tree effects that silently did nothing (the tree rebuild renamed node ids; the shop kept listening for the old names) are re-pointed and live again — frost Deep Freeze / Let It Snow / Sandals, fire More-DoT / Lingering Flame, poison Muscle Degradation / Spreading Death, all four lure poison/paralyze/wider-pull nodes. Frost + poison + lure went from mostly-placebo to functional.
- [ ] **~20 nodes still need NEW effect code** (never built): wet_conductor, wildfire, ricochet, boulder, sharp_rocks_add_bleed, sharper_spiky_bits, shatter, chill_to_the_bone, endless_winter, frost_slow_target, black_lung, crude_oil, patient_zero, thicker_mucus, overload, more_starts, pulse_fire, quiker_pulses, lure_pull_horde_to_spot, black_hole_lure. Element-by-element sessions.
- [ ] **Ultimates gating**: the 6 `ultimate_*` tree nodes exist as data only — the Spellbook still shows all ultimates always. Gate them deep per tree; spikes joins as Rock's ultimate.
- [x] Bleed visual: code-built red drip particle toggled by the `bleed` status.
- [ ] Per-level wizard costs: wire `costs[]` into `wizard_shop_ui`.
- [ ] Magnitude tuning to match the new tree descriptions.
- [ ] **Elemental spiders**: fire spiders ~W3 (immune to fire + prompt), elemental types trickling in from ~W5.

## Enemy polish (parked)

- **Spider:** [ ] attack animation (dug-in spiders currently freeze mid-walk-cycle while gnawing).
- **Snail:** [ ] UV + texture; [ ] fracture shell into shards (progressive cracking = the health bar); [ ] squish-death; [ ] skirt-ripple shader; [ ] tune collision shape.
- **GroundStain system**: [ ] **(idea)** generalize `FluidField`'s stain layer onto flat terrain for snail slime trails (later oil/blood/scorch). Start CPU stain-only on a coarse grid; move to a GPU render-target if scale demands.
- **Frost:** [ ] icicles on the rim; [ ] steam/mist (`FrostFx`); [ ] true ice nucleation; [ ] tuning.
- **Slime:** [ ] reuse the parked `tendrils()` drip; [ ] look tuning.

## Artifacts (scaffold)

- [ ] **(WIP)** Relics dropped from kills → small passive bonuses (right-edge tab/panel). Loop works (drop → grant → bonus); gold_per_kill + tower_max_hp live. TODO: real catalogue, wire queried bonuses (cart_speed / rock_damage / conjure), tower display markers, decide meta-progression persistence.

## Art / style

- [ ] **Stylized art pass**: settle a consistent look once real models exist. Parked toon-shader tech in `VFX/Shaders/toon*.gdshader`. No outlines (faceted / soft-matte).

## Polish / quick wins

- [ ] **Goblin wizard: more animations** — a real Idle (a procedural one already ships unused inside `Goblin.glb`), hat-tip on purchase, ear wiggle (ear bones exist), a "ta-dah" when an item finishes. The archer model in the same folder can reuse the whole rig pipeline.
- [ ] **Grass trample: smooth the pop-back** — blades still rise a touch fast right behind a walker (the just-left spot only ever got the stamp's soft skirt). Ideas: max-blend stamping, ease the shader response (str²), widen the flat core.
- [x] Tower HP bar shrink / reposition.
- [x] Flame VFX: visible ignite/burning on enemies.
- [x] Cart inventory cap → 9.
- [ ] item_bench: fix (spawn frozen dummies) or remove.
- [ ] Tuning pass on the 6 element trees + TAB base tree + lure feel.
- [ ] Day/night cadence (currently 240s), refine later.

## Known bug watch

- [ ] **(WIP)** Restart-run crash (build-only, doesn't repro in editor). Crash logging added (`user://logs/godot.log` bundled into `balance_logs/engine_logs/`); waiting on a captured crash log.
- [ ] Game file size + lag pass (overlaps Perf Phase 0).

## Ship prep (before any public launch)

- [ ] **RBL: replace the Roll-Call assets** (`Imported/RollCall/`: PureNature rocks/grass, JMO CartoonFX textures, skyboxes). CONFIRMED not ours: the packs were bought on a teammate's Unity account (per-account licence, non-transferable), so replacement with owned/CC0 art is required — no licence-clearing path. Inventory: `Imported/RollCall/RBL_ASSETS.md` and [Asset Pipeline](../tech/asset-pipeline.md). Currently in active use: the rock item visuals (6 rocks + atlas — easiest swap, a small Jennifer ask) + placed boulders/grass/VFX textures.
- [ ] Keep the game repo **private** while any RBL asset is tracked in it (pushing raw Asset Store files publicly = redistribution).

---

## Recently done

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
