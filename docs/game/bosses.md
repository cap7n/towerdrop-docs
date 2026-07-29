# Bosses & The Run Arc

*Designed 2026-07-25 (Yaro + Claude session). Status: <span class="pill wip">WIP</span> — **v1 BUILT 2026-07-26** (see the build map at the bottom); needs an in-editor eyeball + tuning pass. This page is the spec.*

*Playtest signal (2026-07-27): the Queen landed well with an outside tester — notably someone who dislikes this genre. First external validation of the boss-arc bet. A **Queen rematch placeholder** now sits at wave 20 (same Queen + spider escort, curated block 2) until Queen II — HP x2, faster eggs — is designed.*

## Why bosses (the problem they solve)

Two findings from the same day forced this page. A tester was **unstoppable by minute 15** — the snowball wins and the game keeps politely serving spiders to a god. And runs are **"1 run and done"**: they don't end, they dissolve, and nothing pulls the player into run 2.

Bosses fix both at once:

- **The run gets an arc.** A boss every ~5 waves turns endless mush into chapters: build toward the wave-10 boss, survive her, rebuild toward 15. The wall an unstoppable player needs isn't more spiders with more HP — it's a boss they *can't beat yet*. That's Vampire Survivors' actual trick (Death at minute 30): the run ends with shape instead of petering out. The "no win condition, runs stay endless" decision survives — a boss wall is a horizon, not a win screen.
- **The meta gets a heartbeat.** "Beat the wave-N boss" is exactly what the achievements → unlocks pipeline (built this week) is starving for. Die to a boss → unlock something → next run reaches further. Bosses are also the leading candidate for the **shard source** — see [Progression](../systems/progression.md).

## Boss 1 — the Spider Queen (wave 10)

Yaro's call: no new creature for the first boss — she's "spider, but wrong," so there is nothing new to teach visually.

| Trait | Spec |
|---|---|
| Body | Spider, **2.5× the largest normal spider**, black (`mesh_tints`) |
| Shield | **50 HP, rock-resistant**: rock hits deal 1, elements deal full. The INVERSE of the existing breakable pure-shield — wave 10 is the exam for "did you build anything besides rock," which the wave-3 teaching wave sets up |
| Summons | Broods spiderlings continuously while alive — **burst of 3-5 every few seconds with a concurrent cap (~30-40)**, NOT a raw 10/s (that fills the 250-enemy perf ceiling in ~25 s). Summoned spiderlings pay **no/low gold** ("starved brood") or the fight becomes the best farming spot in the game |
| Webs | Shoots mounted **artifacts** with web (reuse the web-shooter strand visual): artifact disabled **until wave end** (the spec's "5 min" outlives every wave anyway). v2 counterplay: click the web to tear it free, repair-style |
| Eggs | Lays 1 HP eggs around the tower; each hatches **5 spiderlings after 20 s**. On her death **eggs wither** (pop cascade); already-hatched spiderlings stay and get mopped up — the wave must not end on an egg hunt |
| Phases | **Circles the perimeter once** (webbing + laying), then **climbs and bites: 15 dmg/bite** — big chunks against the 500 HP tower, brick armour soaks first |

### Integration warnings (systems built THIS WEEK will misfire on her)

- **The straggler cull will assassinate her**: `assault_timeout` (120 s) forces REST while she lives, then the cull executes anything when ≤2 are left after 60 s. Boss waves need a `boss_wave` flag — no assault timeout, cull exempts the boss (and probably her eggs).
- **Checkpoint**: saves only fire on a clear REST field, so a mid-boss quit resumes at the wave-10 rest. Correct as-is; just expected behaviour, not a bug.
- **Kill payout**: her own death should pay like a jackpot (existing HP-scaled coin curve does this for free) — it's the brood that must not pay.

## Architecture: ability components, not a boss framework

Neither one-off hardcoded bosses (that's how `enemy.gd` reached 1800 lines) nor a grand framework designed off one example. The middle path already exists in the codebase, built and waiting for its first customer: the **enemy component system** (`EnemyLocomotion` / `EnemyAttack` / `EnemyAbility[]`, opt-in).

- `SpiderQueen.tscn` = the composed enemy path. Her behaviours are **reusable ability components**: `SummonBrood`, `LayEggs`, `WebArtifacts`, plus an `OrbitThenClimb` locomotion phase.
- Each brick outlives her: `SummonBrood` is tomorrow's carrier elite, `WebArtifacts` a future web-shooter upgrade, `LayEggs` any brood enemy.
- **Wave injection is data**: the spawn queue is already specs (`{scene, hp, immune, shield}`) — a boss wave is one more entry in the curated table (W10), and later the generator's "boss every 5 waves" hook.
- Rule of three: build boss #2 from different bricks, and only extract a "boss framework" when boss #3 shows what one actually needs.

## Build order (when construction starts)

1. `boss_wave` flag: assault timeout off, cull exemption, telegraph variant (bigger flare / banner).
2. Queen scene on the composed path: size, tint, shield-with-type-modifier (extend `set_pure_shield`'s pattern).
3. `OrbitThenClimb` + bite (existing climb/attack states do the heavy lifting).
4. `SummonBrood` (burst + concurrent cap + no-gold flag), `LayEggs` (egg = minimal immobile enemy with a hatch timer).
5. `WebArtifacts` (strand visual + `webbed` flag on TowerArtifact: effects skip while webbed, cleared on wave end).
6. Achievement row: "slay the Spider Queen" → unlock; boss HP bar can be v2 (the enemy bar scaled up works for v1).

## v1 BUILD MAP (2026-07-26 — all six steps landed)

Everything on the **composed enemy path** — the component system's first real customer, as planned. New files under `Characters/Enemies/`:

- **`Scenes/SpiderQueen.tscn`** — enemy core (`is_boss = true`, new core export), 60 body HP (tune!), `base_size 8.0` (= 2.5× the largest normal spider's 3.2), black via `MeshTintAbility` ("body" mesh → near-black; the pale fangs and eyes are left for contrast), `HealthBar3D` above her (hidden until first BODY damage — so the bar appearing doubles as the "shield is down" tell; the grey powerup aura = shield phase).
- **`Components/boss_shield.gd`** (`BossShieldAbility`) — 50 HP, all damage lands on it first; the resisted type (rock/PURE) is **capped at 1 per hit**, elements deal full. Rides the composed `modify_damage` chain; grey aura tell, burst + aura-clear on break.
- **`Components/orbit_then_climb.gd`** (extends GroundClimb) — walk in → **one full lap** at `orbit_radius 25` (bearing-accumulated, 90 s failsafe) → walk in, climb, dock. `is_orbiting()` is the window the webbing/egg abilities poll. Lure-leap no-op (too massive); `knockback_dislodge_speed 100` (blasts can't rip her off). Status slows apply on the lap — a frozen Queen stops mid-orbit, deliberate counterplay.
- **`Components/summon_brood.gd`** — 3–5 spiderlings every 4 s while she lives, skipped while total alive ≥ 35 (the perf-cap guard), spawned around HER via the new reinforcement path, **starved** (no coins).
- **`Components/lay_eggs.gd`** + **`Scenes/SpiderEgg.tscn`** + **`Components/egg_hatch.gd`** — up to 6 eggs on the lap; egg = real 1 HP enemy (pays 1 coin if crushed — egg-hunting is rewarded), hatches 5 starved spiderlings after 20 s (wobbles the last 3 s); on her death all standing eggs **wither in a 0.15 s-stagger pop cascade**. `burst_death.gd` is the new generic pop-feedback ability (future bombardier corrode burst).
- **`Components/web_artifacts.gd`** + **`VFX/Shaders/web_cocoon.gdshader`** — up to 3 mounted artifacts webbed per lap. **Yaro's half-sphere net design**: procedural web lattice (polar spokes + rings, wobbled) on ONE hemisphere, the open half discarded; flown **open-side-first** so the landing seats a web CUP over the artifact. `TowerArtifact` gained `webbed` + `set_webbed()` (adopts the net, gates `_wave_live()` so every turret/coin effect stops, self-clears on `wave_completed`). New `tower_artifacts` group; the laser/emitter `_ready` overrides now call `super._ready()` (they silently skipped base registration).
- **Wave side** (`wave_manager.gd` / `enemy_spawner.gd`) — curated **W10 = `[[QUEEN, 1]]`** (she IS the wave; brood + eggs supply bodies). `BOSS_SCENES` marks boss waves: assault timeout suspended while a boss lives (it would have handed her to the straggler cull), cull exempts `is_boss`, telegraph flare turns **blood red** (`boss_signal_color`; the bigger flare/banner stays v2). New `spawn_reinforcement(scene, pos, starved)` — position-spawn through the real pool path, kill-counter connected (brood deaths count for HUD/achievements), `starved` nulls the enemy's coin spawner for that life.
- **Meta** — `Achievements` DEFS row **`regicide`** ("Regicide", placeholder name): kill her once. **Reward deliberately empty — Yaro picks** (shards are the leading candidate). Compendium Bestiary got Queen + Egg rows. Both desc consumers now tolerate goal-1 rows without `%d`.

**Kill payout:** her 60 HP rides the existing HP-scaled coin curve → ~175 gold jackpot, for free.

### First boss playtest (2026-07-26, Yaro — SAME DAY as the build)

**He lost. To the boss. "Pretty difficult love it."** The log: wave 10 = 99 bodies / 246 HP spawned (Queen + brood + eggs), 61 killed, peak 42 alive, tower 395 → 0. The run-arc thesis held on its first contact with a player.

Two bugs found and fixed same day:

- **The wave "completed" the moment the Queen spawned** (Survivor popped, REST + the artifact draft fired mid-boss). Cause: `get_alive_count()` reads EnemyManager's **once-per-frame cache**, built before that frame's spawn — so the frame the spawn queue empties, the cache still says 0 and a one-spec wave declares itself cleared with a live boss on the field. Never bit before because no earlier wave emptied its queue on its first spawn frame. **Fix: a spawn frame can never be a clear frame** (`_tick_assault` returns after `_release_one`). ⚠️ Rule: anything gating on `get_alive_count()` the same frame something spawned is reading a stale count.
- **Egg hatches/withers counted as kills**: `release_enemy` always emitted `enemy_released`, and every listener treats that signal as "a kill happened" (Lucky Coin gold, drop pity, beam cooldown, BalanceLogger throughput). `release_enemy` grew a `silent` flag; hatch + wither use it. Related connection leak fixed in passing: the kill-counter was a per-spawn `CONNECT_ONE_SHOT` on `health_depleted`, which leaks on bodies that pool WITHOUT dying (hatched eggs) and would double-count their next life's death — now ONE permanent meta-guarded connection per body (`_hook_kill_counter`), shared by the drip and reinforcement paths.

Known-and-accepted: **Lucky Coin still pays on starved brood kills** (it hooks `enemy_released`, not the coin drop) — an earned relic paying a trickle during the fight is fine; the log showed ~200 gold across the whole lost fight vs ~350+ in normal waves 8-9. Brood kills also advance the drop pity — also fine (artifact supply shouldn't stall during a 2-minute boss).

### Still open (v2 / tuning)

- Numbers pass in the editor: body HP 60, shield 50, bite 15 dmg / 2 s, orbit radius/speed, brood cadence + cap 35, egg count 6, webs 3. All exports on the scene's component nodes.
- Web tear-off counterplay (click the cocoon, repair-style) — spec'd, not built.
- Bigger telegraph (banner / double flare) — flare recolor only for now.
- Ragdoll death at scale 8 may read comical — eyeball; a bespoke death (collapse + brood scatter) is a v2 candidate.
- Passive artifact bonuses (cart speed etc.) are NOT stopped by webbing — only `_wave_live`-routed effects (turrets, coin maker). Deliberate v1 scope; revisit if webs feel toothless.
- Lure quirk: she can be baited during the walk-in (delays her, reads fine); a lure held near the wall can pull her past the orbit trigger and skip the lap. Watch in play.
