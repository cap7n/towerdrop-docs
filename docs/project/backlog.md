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
- [ ] **Settings**: missing master/music/sfx volume (+ fullscreen).
- [ ] **(WIP)** Tester feedback channel: in-game note (press C) exists; still want a Discord/report link visible.

### Tier 4: looks-finished polish

- [ ] **(WIP)** Replace placeholder cubes (electric / frost / lure wizards): goblin wizard being modelled (purple-hat low-poly, no-outline). One base recolored per element + conjure/idle anim.
- [x] Confirm hit feedback is clear without damage numbers: body hit-flash only.
- [ ] Tower HP bar shrink / reposition; HUD readability pass (wave / HP / gold / loadout).
- [ ] Audio: key SFX (hit / buy / ultimate / wave-warning) + music balance.
- [ ] **Perf Phase 0 quick wins** (shadows, alloc flood, ragdoll caps) so wave 10 doesn't chug.

---

## Tower ultimates

- [x] **Rock: Oil the tower** (oil coat + fire-wave). Key `O` / `L` ignite.
- [x] **Frost: Freeze the tower** (ice rocks + ice wall coat). Key `I`.
- [x] **Poison: Slime the tower** (jiggly mucus, slow+poison). Key `P`.
- [ ] **(idea)** Do Fire / Electric / Lure want their own tower ultimates too? (6-element set)
- [ ] Tower ultimates gated **deep in the wizard tree** rather than the always-on spellbook.

## Bigger features (later)

- [ ] **(WIP)** **Tower-damage rework**: spiders dig in & attack at a random wall height. Pass 1 done (band 40→95% of top, `attack_height_min/max_frac`). TODO: HP/damage rebalance.
- [ ] **Visible tower damage**: cracks / chipped bricks / debris / damage states as HP drops (real feedback for dig-in). *(Foundation now exists: the brick-shell damage-state system, see [The Tower](../game/tower.md).)*
- [ ] **Buildable defenses**: satellite towers + traps (TAB-shop → build tray → click-to-place). Iter-1 exists but free / no purchase.
- [ ] **Traps**: more placeable ground hazards (caltrops / grinder / conveyor / chain); new plan: buy at a wizard then throw into place (reuse aimed-throw).
- [ ] **(WIP)** **More enemy types**: scripts done (thin `extends Enemy` subclasses); each needs a model + scene:
    - [x] **Pillbug**: done + feels good (curl-morph roll, charge-bonk-stun ability). Polish: real leg-wiggle shape key, UV texture, wire into waves. Redesign: replace slow float-drop with a thrown "shot".
    - [ ] **Termite**: fast ~1HP swarm, chews wall fast.
    - [ ] **Bombardier**: death-cloud corrodes the tower; resists poison.
    - [ ] **Carrier**: always hauls an item, tankier.
    - [ ] **Centipede**: head tows follower segments; first pass exists. TODO: per-segment colliders, mid-split, status propagation.
    - [ ] Per-element resistance/weakness system (quick-test version exists).
- [ ] **Flying enemy (moth)**: parked on the anti-air question; answer agreed: aimed-throw = skill-shot AA + an AA tower upgrade. Build after aimed-throw feel is locked.
- [ ] **(WIP)** **Enemy component system**: built (`EnemyLocomotion` + `EnemyAttack` + `EnemyAbility[]`), opt-in inside `enemy.gd`. TODO: A/B `Spider_v2` vs `Spider`, port Snail, migrate the 5 subclasses, then delete the legacy inline path.
- [ ] **Jumping spiders**: leapers.

## Elemental interactions / combos

One element sets up (a coat/status), another pays off (a trigger).

- [x] **Oil + Fire = ignite** (fire spreads a burn-wave on the oil slick).
- [x] **Frost + Rock = brittle** (frozen enemies take 2×, currently all damage; scope to rock-only later).
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

## From playtest C4 (wave 39+ marathon)

- [ ] **(WIP)** **Kill the snowball**: went unkillable past wave 10 (spikes insta-kill + regen heals all back). Done: removed spikes / poisoned spikes / HP regen from the TAB tree (systems kept). Next: budget wave generator, late-game tuning.
- [ ] **Spikes → Rock-wizard spell** (costed/upgradeable), not an always-on TAB passive.
- [ ] **HP regen → upgradeable spell**, not a forever passive.
- [ ] **Frost is underpowered**: concept: snow-storm on hit (slow + DoT AoE), "avalanche" upgrade.
- [ ] **Fire & lightning too cheap vs rock**: element cost rebalance.
- [ ] **BUG: lightning's spread fires on ROCK kills**: element effect bleeding across equipped wizards.
- [ ] **Roller (pillbug) aiming is un-fun**: make the path visible or the orbit predictable.

## Shop / trees: Pass 2 wiring

- [ ] Sharp Rocks → bleed (hook the node to the already-correct bleed status).
- [x] Bleed visual: code-built red drip particle toggled by the `bleed` status.
- [ ] Chilly Aura: frozen enemies emit a slowing AoE.
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

- [x] Tower HP bar shrink / reposition.
- [x] Flame VFX: visible ignite/burning on enemies.
- [x] Cart inventory cap → 9.
- [ ] item_bench: fix (spawn frozen dummies) or remove.
- [ ] Tuning pass on the 6 element trees + TAB base tree + lure feel.
- [ ] Day/night cadence (currently 240s), refine later.

## Known bug watch

- [ ] **(WIP)** Restart-run crash (build-only, doesn't repro in editor). Crash logging added (`user://logs/godot.log` bundled into `balance_logs/engine_logs/`); waiting on a captured crash log.
- [ ] Game file size + lag pass (overlaps Perf Phase 0).

---

## Recently done

- Game renamed → **"Tower Drop"**; menu patch-notes panel (fed from `CHANGELOG.txt`).
- Spellbook ultimates (Oil/Frost/Slime, 15g/10s) that actually affect enemies.
- Fire pass: stylized flame, ground-impact ignite + enemy-to-enemy spread, Oiled+Fire torch.
- DEBUG SANDBOX panel (F4) replacing scattered debug keys.
- Wizard shop: Prev/Next wizard arrows, hover tooltips.
- New modeler tower imported + brick-shell MultiMesh with runtime damage states.
