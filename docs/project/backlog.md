# Backlog / Design record

!!! note "The live task list moved to the board"
    Day-to-day tasks (priority, difficulty, roles, milestones, claim-lock) now live on the **Ingenui Tasks** board, in the **Tower Drop** project. Add, claim and close work there. This page is no longer the checklist; it is the **design record**: the reasoning behind unbuilt ideas, the open questions, and the log of what shipped. When an open question here gets decided it becomes a task on the board.

**Status tags (used in the history sections below):** <span class="pill done">Done</span> shipped · <span class="pill wip">WIP</span> in progress · <span class="pill idea">Idea</span> undecided.

**Roadmap:** the versioned plan lives on the [0.1x roadmap](roadmap.md); the board's milestone labels (`0.11`..`0.18`, `Steam launch`) mirror it.

---

## Design sketches (captured, not yet built)

These hold the thinking that the terse board cards can't. Each is also an **Open Question** on the board; decide it there and it becomes real work.

### Endless mode — "test your accumulated power"

*(Yaro, 2026-07-27.)* An **extra, optional mode alongside the main game — not a new direction, and it changes nothing about the normal campaign.** Picked from the menu next to a normal run; its own arena. The main game is *already* endless with no win state ([Decisions](decisions.md)) and already has a wave-11+ threat-budget generator, so the point of THIS mode isn't "endlessness" — it's the **dedicated arena + framing**: bring a fully-stacked build and see how long it holds against a continuous flood, with no curated 1–10 arc to sit through first.

- **Core twist — NO ground, tower only.** Strip the ground plane and the tower BASE entirely: no terrain to walk across, no approach phase. The tower floats on its cloud-island and you defend it top-to-bottom; enemies are on the wall from the moment they appear (climb-only). Leans straight into the core climb mechanic and drops the surrounding-arena parts of `level_1` (`Terrain`, `TowerBase`, the ground walk).
- **Setting:** the tower on a **mountain peak in a sea of clouds** — a high-altitude island, cloud instead of ground. Reuses the horizon trick (flat `WorldBackdrop` + fog) plus a cloud-sea layer and the tunable `TD_Sky_01`. The clouds ARE the floor.
- **Open — where do enemies come from?** With no ground they can't walk in. Candidates, undecided: (a) rise out of the cloud sea at the tower foot and climb; (b) emerge already-climbing from gates on the tower; (c) drop / fly in from above. This departs from the core fiction (enemies streaming across terrain), so it needs deciding, not assuming.
- **Loop:** continuous survival, pressure via the existing threat-budget + swarm levers, within the locked [200–250 concurrent cap](../pillars.md).
- **Still open overall:** separate scene vs a mode flag on `level_1`; what "accumulated power" means at entry (start maxed, or build up in-mode); how the mountaintop reads with the wave-direction windrose. Scope is large, so likely post-0.1x if pursued.

### Walled settlement — a separate map

*(Yaro, 2026-07-27.)* Explicitly **NOT** the endless mode above: a **different map, with different enemies**, where the tower / settlement is **ringed by walls and enemies attack through gates**. The walls-and-gates fiction belongs HERE, not in the cloud-island mode. Everything open: what the settlement is (multiple structures? tower plus outbuildings?), which enemies are unique to it, whether it's another endless mode or a distinct scenario, and how walls / gates change cart positioning and enemy approach.

> **More undecided forks live as Open Questions on the board:** win condition + run-summary screen, a mana currency, artifact-upgrade cards in the draft, thief flyers, fire/electric/lure tower ultimates, a mix-2-specializations wizard tier, the beam re-acquisition-lockout rework, difficulty-as-challenges, and a water element / fluid.

---

## Locked design decisions (quick reference)

Full rationale in the [Decision Log](decisions.md). The load-bearing ones for day-to-day work:

- **Enemy cap 200–250**, smooth on most machines — not thousands. Preserves the feel-rich per-enemy effects. ([Pillar 5](../pillars.md).)
- **No win condition** — runs stay endless. A run-summary screen is still wanted (open question).
- **Recipe / auto-combine system fully deleted** (not parked). The enemy **carry** system was kept.
- **Spider HP cap = 15.** A maxed rock can't one-shot a late spider, but frozen enemies take 2×, so **frost is the setup tool** back into one-shot range. Keep `frozen` at 2×-all — it's load-bearing.
- **Breakable pure-shield** (rock chips it, elements bypass), introduced ~wave 15 = "rock immunity" in breakable form.
- **Generator budget buys variety + powerups, not raw HP** — elemental immunities and shields, resistances assigned deliberately (not randomly).
- **Snowball killed by removing always-on TAB passives** (spikes / poison spikes / HP regen removed from the TAB tree; systems kept for re-wire as costed spells).

### Elemental interactions (the combo matrix)

One element sets up (a coat / status), another pays off (a trigger). Design reference; the wiring status is a task on the board.

- **Oil + Fire = ignite** — done (fire spreads a burn-wave on the slick).
- **Oiled enemy + Fire = instant torch** — done (bigger burst, fiercer burn).
- **Frost + Rock (or Electric) = brittle** — done, scoped 2026-07-17: frozen amplifies the shatter types (PURE + ELECTRIC) ×2, Deep Freeze ×4. The frost→rock one-shot the HP-cap design leans on still works.
- **Electric + Oil/wet = arc conductor** — chain lightning jumps further to oiled neighbours (`wet_conductor` gives oiled links; the first-class combo is a board task).
- **Ideas:** Frost + Fire = thermal shock; Slime/Poison + Fire = toxic flare; Frozen + heavy hit = shatter shrapnel; Lure = the universal setup.

---

## Build notes — shipped systems

Kept as a design record because the *shape* is reference even though the work shipped. Implementation detail lives in git and the systems pages; polish / tuning follow-ups are tasks on the board.

- **Two-layer tower HP + repair.** The 110-brick shell is an ARMOUR layer (10 HP/brick, local coverage radius) that soaks hits before the core HP bar; bricks tint, crack and tumble off. Click-to-repair (hammer cursor) is a **1-HP-per-click rate** you upgrade (`bonus_hp_per_click`), not an instant full heal. Buried underground bricks are excluded from the pick + soak. See [The Tower](../game/tower.md).
- **Save system (0.10).** Per-profile (three fixed slots) + a REST-checkpoint run save, menu **Continue**, meta.json that survives game over (banks playtime / achievements / meta points). Apply order is load-bearing.
- **Achievements + Compendium.** `Achievements` autoload: bare-fact reporting (`bump` / `raise_to`) + a DEFS data table turning stats into unlocks; def-based artifact gating; Steam-style popup card; full-screen Compendium (Achievements / Artifacts / Bestiary / Spells).
- **Meta progression (started 2026-07-27).** Death screen leads with banked **meta points** (1/wave, +10 boss) + a tree frame (nodes still to design in `skill_tree_maker.html`). **Role split:** the meta tree is permanent stat boosts only; artifacts stay achievement-unlocked; the tree also gates whether a wizard can learn spells.
- **Artifacts.** Placeable `TowerArtifact` relics mounted via ghost placement (hang = side wall, top = tower top); effects run only while mounted during live waves; after-every-wave 3-card draft; brick-link knock-off; bag grid + Mover tool. A pity-timer governs kill drops.
- **Spellbook + tower ultimates.** A non-modal two-page book with element ribbon tabs; a spell casts only once a wizard owns its `ultimate_*` tree node (plus an achievement gate). Castable: Oil, Freeze, Slime, Meteor Shower, Living Stone.
- **Ground fluid v2 (validated 2026-07-26).** The WALL keeps the polar field; the GROUND got its own **Cartesian XZ sim grid** (uniform cells at any radius) with two one-way bridges at the foot (oil leaks down; ground fire ignites the wall). One ground-surface system that also becomes the home for future stain channels (slime / blood / scorch). The original design rationale is preserved below.

??? note "Ground fluid v2 — original design rationale (for the record)"
    The fluid fire proved the direction; the flaw was the coordinate system: the field is POLAR (columns around the tower), so ground cells stretch with radius — same splash, three sizes. The redo:

    - **Split the surfaces.** The WALL keeps the polar field (angle × height is natural there; coat mechanics don't change). The GROUND gets a **Cartesian XZ sim grid** — uniform cells everywhere, any radius. Same architecture as the trample map (world-space square around the tower).
    - **Two one-way bridges at the foot:** oil off the wall's bottom row deposits into ground cells beneath it; ground fire at the foot can ignite the wall's bottom row. That's the whole seam.
    - **API survives, internals swap:** `splash_ignite` / `ground_hazard` / walker sampling / black lung all route ground points to the new grid — call sites untouched.
    - **The real prize — ONE ground-surface system:** the grid IS the GroundStain idea (snail slime, blood, scorch, oil) — every ground mark becomes a channel in one sim instead of four bespoke systems.
    - **Perf watch:** the sim is a per-frame CPU loop; measure before committing to resolution — this must not un-win the perf war.

---

## Done log

### Recently done

- **2026-07-19 (v0.017, part 2 — the ARTIFACT sprint):** artifacts went scaffold → **full physical system in six passes**: placeable `TowerArtifact` relics mounted via ghost placement on two surface types; effects run only while mounted during live waves; all 6 artifacts live; stacking additive-from-base; the ✦ bag is a 3×3 icon grid; **Mover tool**; brick-link knock-off. New plumbing: `wave_completed` signal, `spawn_bonus_coin`, collision layer 8.
- **2026-07-19 (v0.017):** **SPELLBOOK v1** — an actual two-page book with **tree-gated spells**; **Meteor Shower** built; rocks CRUMBLE on impact. Cleared Cap7n's high-prio list: wizard **Stats screen**, **Wizards button** in the TAB shop, **windrose** compass. Sister's QoL: F4 panel moved, click-outside closes menus. **Conjure timers** toggle. **Cart v2 model**. Win condition + Mana logged as open questions.
- **2026-07-18 (post-v0.016 playtest round):** **Auto next wave** checkbox; **element switching redesigned** (the tree's base node is a "Change element" doorway); **wizard profile cards**; **lure redesign** (plain item + AoE, glowing orb). Backlog consolidated into this wiki page.
- **2026-07-16/17 (v0.016):** Two-layer tower HP + **Repair tool**; **Settings menu**; Rock-first element gate as a standalone rule; Frozen scoped to PURE+ELECTRIC; wizards face outward; grass-trample cache-eviction fix; environment polish; game icon; **ShaderPrewarm**; BalanceLogger fps/worst-frame + cheat-taint columns. Shipped to testers; two logs analysed same day.
- **2026-07-12:** Goblin wizard LIVE (rigged + Conjure loop + per-element outfit recolor); 13 orphaned skill-tree effects reconnected; spikes redefined as a Rock ultimate.
- **Living grass:** dense tinted grass + wind sway shader + trample map (enemies part the grass, 60 s spring-back trails).
- Game renamed → **"Tower Drop"**; menu patch-notes panel (fed from `CHANGELOG.txt`).
- Spellbook ultimates (Oil/Frost/Slime); fire pass (stylized flame, ground-impact ignite + spread, Oiled+Fire torch); DEBUG SANDBOX panel (F4); New modeler tower + brick-shell MultiMesh.

---

## Playtest archive

_Playtest and feedback logs, kept for the record. Live follow-ups from these are tracked on the board._

### High priority — from playtest (2026-07-17, Cap7n)

- <span class="pill done">Done</span> **Remove the debug aim line** (2026-07-18: the magenta beam + spawn call deleted from cart.gd).
- <span class="pill done">Done</span> **Wave direction indicator hidden by grass** (2026-07-19): a standalone HUD **windrose** top-centre; arrow points at the incoming sector, pulses during telegraph.
- <span class="pill done">Done</span> **Show money invested per wizard** (2026-07-19): the wizard shop's **Stats screen**.
- <span class="pill done">Done</span> **Jump from the TAB shop straight into a wizard's tree** (2026-07-19): a "Wizards" button hands off to the wizard shop.

### Feedback (2026-07-19, fellow game dev)

New players spend attention decoding systems instead of fighting the horde. **→ Grew into the [⚡ Revamp](revamp.md) plan:** tutorial slows down as its own mode, the main game speeds up with VS-style in-your-face choices.

- <span class="pill done">Done</span> **Separate the tutorial from the main game** — Tutorial is now its OWN mode (start-menu entry; the main game starts clean). ⚠️ **The tutorial CONTENT itself is not done** — the separation/plumbing landed; the guided walkthrough is a board task (0.14).
- Systems overload, and "telegraph that upgrading exists": design themes, now board tasks (progressive unlocking; rest-phase nudges; affordable-upgrade glow — the wizard signal tab shipped part of this).

### From playtest (2026-07-26, Cap7n, boss build day — fire feel)

- <span class="pill done">Done</span> **Pipe bomb felt like a DOWNGRADE** — fire items now detonate on first contact and seat a burning patch at the blast.
- <span class="pill done">Done</span> **"I have to MISS to get the fire zone" → FIRE IS OIL NOW** — any fire that would leave a patch deposits burning FUEL into the FluidField and lights it (`splash_ignite`); one `FireEffect._leave_fire` seam. Ground-walkers now sample the field (`ground_hazard`).
- <span class="pill done">Done</span> **Burning oil was a flat colour** — now runs the two-tap scrolling-noise flame technique, reusing the bound `warp_noise`.
- <span class="pill done">Done</span> **Boss HP bar removed** — the Queen reads like every other enemy (grime + shield aura). `HealthBar3D` kept unused for a future proper boss bar.

### From playtest (2026-07-25, Cap7n, post-0.10 build)

8 waves / ~8.5 min / 191 kills. The log showed what the notes didn't: the tower took **53 damage across 8 waves** (waves 1–3 zero) — the early game is untouchable — and **62% of gold went unspent**. Perf clean (~48 fps, peak 31 alive).

- <span class="pill done">Done</span> **Shop "signal" tag** — `signal_node: true` flashes the right-edge tab that OPENS the shop (a throbbing red "!" badge; scale reads in peripheral vision, brightness didn't). Only Hire Wizard is tagged.
- <span class="pill done">Done</span> **Rest auto-started even with "Auto next" OFF** — with Auto next OFF rest now lasts until Enter; a **straggler cull** (≤2 left after 60 s) replaces the old force-start.
- <span class="pill done">Done</span> **Wizard hiring needs a visible entry point** — a third right-edge pull tab, pulses when the next wizard is affordable.
- <span class="pill done">Done</span> **Beam artifacts too strong** — a kill costs a 3 s recharge before re-target.
- <span class="pill done">Done</span> **Too many artifacts from drops** — replaced with a **pity timer** (chance starts at zero, each kill adds a step, a drop resets it). ~1.2 drops/run at the default step.
- <span class="pill done">Done</span> **Unlocks applied to the CURRENT run** — `Achievements` snapshots the unlocked pool at run start; the popup reads "unlocked for your next run".
- <span class="pill done">Done</span> **BUG (serious): enemy hit-flash + damage grime wiped permanently** — the status system handed `HitFlash`'s `material_overlay` slot back as `null`, poisoning pooled bodies. Fixed via `base_overlay()` + `reapply()`. ⚠️ **Rule: anything borrowing `material_overlay` on an enemy must restore `base_overlay()`, never null.**
    - <span class="pill done">Done</span> **Statuses and hit feedback now STACK** via `next_pass` on OUR per-enemy material (never the shared pack `.tres`). The pattern for any "two effects on one body".
- <span class="pill done">Done</span> **Game-over screen shows what the run unlocked** — achievements + artifacts earned this run, with a hover line.

### From playtest (2026-07-17, Jen, v0.016 build)

5 waves / ~9 min. Loved the brick armour: *"i love how the tower bricks fall off btw!"* Discovered artifacts + ultimates on her own.

- <span class="pill done">Done</span> **BUG: Repair mode blocked ALL other clicking** — repair only consumes clicks that hit a damaged brick; UI clicks pass through.
- <span class="pill done">Done</span> **BUG: lure dropped a clump of climbers "stuck but alive"** — redesigned: the lure is now a PLAIN dropped item with the lure AoE attached (`LureBeacon` deleted). New hook: `DroppedItem.linger_hold`.
- <span class="pill done">Done</span> **W far from a goblin did nothing** — now flashes a "no wizard in reach" hint.
- <span class="pill done">Done</span> **"I misunderstood repair wall"** — button + tooltip state the rule.

### From playtest (2026-07-17, Jennifer's sister, v0.016 build)

26 min / 12 waves / zero errors. Economy data cheat-tainted (used F4 gold — BalanceLogger now flags that).

- <span class="pill done">Done</span> **"Double tower coats causes some lag"** — first-use shader-compile stutter addressed with the **ShaderPrewarm autoload**.
- <span class="pill done">Done</span> **Hold-to-pour should stop on release**; **click-outside-to-close menus**; **F4 panel overlapped the inventory** (moved) — all done.
- <span class="pill done">Done</span> **"Can I save my progress?"** — answered by the save system (REST-checkpoint saves + menu Continue).
- **Idea pile** (raw, uncurated): water element · wizard idle/flavour animations · intro/milestone cutscenes · wizard-slot count as difficulty · pricier ultimates late · biomes/levels · skins · chicken mob · ranged mobs that shoot up at the tower · surprise chests · leagues/clans · music intensifies as waves ramp · cherry blossom trees.

### From playtest (2026-07-12, Cap7n)

- <span class="pill done">Done</span> **Tutorial: force ROCK as the first wizard pick** — reworked into a standalone per-run rule (non-Rock elements lock until the run's first Rock tree purchase).
- <span class="pill done">Done</span> **Wave-skip (Enter) → wizards instantly finish their conjure**.
- <span class="pill done">Done</span> **BUG: grass trample stops working on wave 2+** — fixed twice (coverage ±160 m; cache-eviction strong-ref + re-wire on level entry).
- <span class="pill done">Done</span> **REPAIR system (hammer cursor)** — became the two-layer HP system.

### From playtest C4 (wave 39+ marathon)

- <span class="pill done">Done</span> **Kill the snowball's worst offenders** — removed spikes / poisoned spikes / HP regen from the TAB tree (systems kept). Late-game tuning continues as board tasks.
- <span class="pill done">Done</span> **HP regen → upgradeable spell (Living Stone)** — a cast (15g, 10s) switches on `TowerCore.set_regen()`; it STACKS across Rock wizards (sum when the stat IS the payload, max when it's a dial on a shared field effect). The dead always-on `_apply` case is deleted.
- Frost underpowered · fire & lightning too cheap vs rock · roller aiming un-fun · lightning spread fires on ROCK kills · spikes → their own timed spell (mind the `spiks`/`posioned_spikes` id mismatch): all tracked as board tasks.

---

## Codebase cleanup — method note

Dead-code sweeps use **`tools/deadscan.py`** (plain `python`): it strips comments (so a stale doc comment can't make a dead var look alive) but KEEPS strings (this codebase calls across nodes by name), reporting members with zero references. Re-run at each milestone boundary. **A script can't find a comment that lies** — the stale-docs sweep is a per-subsystem read, tracked on the board. Pass 1 (2026-07-25) cut 85 → 38 unreferenced declarations; the worst three were *misleading, not merely unused* (`wave_manager`'s `base_wave_size`/`wave_growth`/`max_wave_size` looked like the difficulty knobs while waves actually come from the curated list + generator).
