# Roadmap & Priorities

The through-line. For the granular task list, see the [Backlog](backlog.md).

**The prototype phase is over.** The 0.0x line (v0.014 → v0.017) proved the game; from here the project runs on a **versioned milestone roadmap**: the **0.1x alpha series**, ending at **0.20 = the Steam private-beta phase** (builds distributed via Steam keys / a private branch to a hand-picked circle; the store page can stay hidden). Dates are deliberately absent — a milestone ships when its theme is playable in a tester build.

## Versioning rules (start at 0.10)

Decided 2026-07-24 (Yaro):

- **Scheme = `MAJOR.MINOR.PATCH.BUILD`** (semantic versioning + Windows build digit; adopted 2026-07-24, Yaro). **MAJOR**: release era — `0` all through alpha/beta, `1.0.0.0` on release day, bumps again only for sequel-scale reworks. **MINOR**: the roadmap milestones — the 0.**10** → 0.**11** → 0.**20** series lives here. **PATCH**: hotfix on a shipped milestone (same content, repaired): tester-breaking bug in 0.10 → ship `0.10.1`. **BUILD**: normally `0`; only for a re-export with no code/content change (export-setting tweak, repack). Rule of thumb for big-vs-small: players must relearn something or saves change shape → MINOR; it just plays better/correct → PATCH.
- **Short form everywhere humans read it, four digits only where Windows demands it.** `config/version` in project.godot, the menu label (auto-reads the setting since 2026-07-24), git tags and `CHANGELOG.txt` headers all use the short form (`0.10`, `0.10.1`, `1.0`); the export preset's `product_version` + `file_version` pad to four (`0.10.0.0` — Godot validates both fields as x.x.x.x, filled 2026-07-24). Steam ignores all of it (own build IDs) — the number exists for humans, crash logs and "which zip are you running?".
- **Every shipped tester build gets a version.** Bump `config/version` **and** the two export preset fields together; the menu version label and a `CHANGELOG.txt` entry ride along as before.
- **Git tag per release**: tag the release commit `v0.10`, `v0.11`, … in the game repo.
- **Engine pinned per release**: currently **Godot 4.7** (`config/features`). Engine upgrades happen only at a version boundary and get recorded here — never mid-milestone.
- **Numbers are themes, not dates.** Content can slip between milestones; the numbering never reorders. A theme that lands small may merge into its neighbour (we'd rather skip a number than ship a hollow one).

## The 0.1x series — alpha, milestone by milestone

Each milestone names its theme, its goal, and the backlog items that define "done". Within any milestone the [pillars](../pillars.md) tie-breaker applies: **base-game fun and feel first**. Every bullet + the milestone heading itself carries a status pill (mirrors the [Backlog](backlog.md)'s system): <span class="pill done">Done</span> shipped · <span class="pill wip">WIP</span> in progress · <span class="pill todo">Todo</span> not started · <span class="pill idea">Idea</span> undecided. A milestone's own pill is the roll-up of its bullets — Done only once everything under it is.

### 0.10 — A game you can put down <span class="pill done">Done</span> *(closed 2026-07-25)*

*The "left prototype" build: runs persist, the build is clean.*

- <span class="pill done">Done</span> **Save system v1, per profile** — the full plan lives in the [Backlog](backlog.md) (Bigger features): REST-checkpoint saves (only when the field is clear — no enemy presence), `SaveSystem` autoload, named profile slots (`user://profiles/`), menu **Continue**, game-over deletes the run file. The Profiles button stops being a stub. **BUILT 2026-07-24, both phases** — profile layer (SaveSystem autoload + three-card Profiles screen + playtime banking) AND the run checkpoint (REST field-clear saves, full state gather/apply, menu Continue per profile, coin-pile reseed). Details in the Backlog entry.
- <span class="pill done">Done</span> Clean **restart after game-over** without relaunching — CLOSED 2026-07-25 (Yaro's call): no repro across the whole 0.10 cycle's heavy churn; the old build-only crash is treated as a long-gone edge case. Crash logging stays in place in case it ever resurfaces.
- <span class="pill done">Done</span> Build hygiene leftovers: cheat keys folded into the F4 sandbox — the LAST raw key (hold-F3 coin pour) folded 2026-07-25: the panel's Gold buttons now pour REAL RigidBody coins onto the pile (Yaro's call — the counter follows as the pile absorbs, so cheat gold behaves like earned gold; `CoinSpawner.pour_coins()`) · test scenes confirmed excluded from exports (2026-07-24).
- <span class="pill done">Done</span> First build stamped per the versioning rules above — `config/version` + the export preset's File/Product Version fields are set to `0.10` (2026-07-24); CHANGELOG v0.10 written 2026-07-25. Remember: tag the release commit `v0.10` when the zip is actually cut.

### 0.11 — The meta loop <span class="pill wip">WIP</span>

*Losing a run still moves you forward.*

- <span class="pill wip">WIP</span> **Meta layer** (`meta.json`, survives game over): playtime, achievements tracker v1, artifact shards. **Achievements tracker v1 BUILT 2026-07-25** (`Achievements` autoload, data-table DEFS, popup card, full-screen Compendium — see the Meta-progression section of the [Backlog](backlog.md)); shards still open.
- <span class="pill wip">WIP</span> **Artifact meta-unlocks live**: the draft, kill drops and debug grant all draw only from the profile's unlocked pool via `Achievements.is_artifact_unlocked()` (def-based gate, no seeding — LIVE 2026-07-25; Sharp Edge + Thick Bark are the first gated ids). The achievements route works end to end; playtime + shard routes still to build.
- <span class="pill idea">Idea</span> **Decide the shard source** (elite drops? wave-clear bonus? game-over conversion) — the one open design call blocking this milestone.
- <span class="pill wip">WIP</span> Profiles screen shows meta progress — the cards show playtime + "Achievements n / total" (2026-07-25); shards join when they exist.
- <span class="pill todo">Todo</span> **Ultimates gated by achievements** (Yaro 2026-07-25): an `"ultimate"` reward key in the DEFS table + a check at the spellbook gate.

### 0.12 — Every system does what it says <span class="pill todo">Todo</span>

*No dead items, no placebo nodes.*

- <span class="pill todo">Todo</span> **Item → effect coverage**: every buyable/usable item does something.
- <span class="pill todo">Todo</span> The **~20 skill-tree nodes with no effect code** (wet_conductor, wildfire, ricochet, boulder, shatter, …) — element-by-element sessions.
- <span class="pill wip">WIP</span> **Ultimates completed**: Meteor Shower is v1 castable; FireWave / Arc Storm / Black-hole Lure effect code, spikes folded in as a Rock spell, per-spell costs, and the wizard-buff inheritance decision are still open.
- <span class="pill todo">Todo</span> **Elemental combos**: Electric + Oil (arc conductor) — the next nearly-free one.
- <span class="pill todo">Todo</span> Per-element enemy resistance/weakness system promoted from quick-test to real.

### 0.13 — The horde <span class="pill wip">WIP</span>

*The enemy roster and the long game.*

- <span class="pill wip">WIP</span> **Enemy component system migration** — `EnemyLocomotion`/`EnemyAttack`/`EnemyAbility[]` built and opt-in; A/B `Spider_v2`, port Snail, migrate the 5 subclasses, delete the legacy inline path all remain.
- <span class="pill wip">WIP</span> New enemies wired into waves: subclass scripts exist for **termite, bombardier, carrier** but none has a model/scene yet; centipede has a first pass (segment colliders, mid-split still open); **turtle / fly / worm** models are wired in but undesigned/unbehaviored.
- <span class="pill todo">Todo</span> **Elemental spiders** (fire ~W3, trickle from ~W5) and the "only THIS element penetrates" layer.
- <span class="pill todo">Todo</span> **Waves beatable to ≥10** tuning + late-game generator tuning (the C4 marathon list: frost buff, element cost rebalance, roller aim). *(The snowball itself is already killed — spikes/regen removed, the budget generator + spider HP cap are live — this is the fine-tuning pass on top.)*
- <span class="pill todo">Todo</span> Revamp Track 2 leftover: **make early choices HIT** (cheap dramatic first tiers, re-tune waves 1–5).

### 0.14 — Onboarding <span class="pill todo">Todo</span>

*A stranger can learn the game without us in the room.* (Deliberately after 0.12/0.13 — the [Revamp](revamp.md) parked the tutorial until the systems it teaches stop moving.)

- <span class="pill todo">Todo</span> **Tutorial mode finished**: the full Track 1 curriculum (Blocks 0–5, 10–15 min, no game-over pressure), completion flag, first-launch nudge.
- <span class="pill todo">Todo</span> **Controls reference** card + quick single-screen tutorial.
- <span class="pill todo">Todo</span> Revamp Track 2 leftovers: **telegraph upgrades loudly** (gold-nudge, glowing shop entries) and the **system reveal order** pass.
- <span class="pill todo">Todo</span> Tooltip / description copy fill + spellcheck.

### 0.15 — Runs smooth <span class="pill todo">Todo</span>

*Wave 10+ doesn't chug on a tester's machine.*

- <span class="pill todo">Todo</span> **Perf Phase 0** quick wins (shadows, alloc flood, ragdoll caps — see [Performance](../tech/performance.md)); sustained-cost profiling (double tower coats / FluidField).
- <span class="pill done">Done</span> **Restart-run crash** — CLOSED 2026-07-25: never reproduced again across the 0.10 cycle; treated as a long-gone edge case. Crash logging (`user://logs/godot.log` bundled into `balance_logs/engine_logs/`) stays in place should it resurface.
- <span class="pill todo">Todo</span> Game file size + load-time pass.

### 0.16 — Looks & sounds (polish I) <span class="pill wip">WIP</span>

- <span class="pill wip">WIP</span> **Audio completion**: `Sfx` autoload built and wired for clicks/coins/skill-unlock/spider-bite/pillbug-slam/wall-repair; still missing ultimate casts, item impacts, enemy deaths, wave-warning tie-in, and the music-intensity ramp.
- <span class="pill todo">Todo</span> **HUD readability pass** (wave / HP / gold / loadout) + menu fog tuning.
- <span class="pill todo">Todo</span> VFX debts: magical **aim-line** treatment, **per-element beam identities**, grass pop-back smoothing.

### 0.17 — Art pass (polish II) <span class="pill todo">Todo</span>

- <span class="pill todo">Todo</span> **Stylized look settled** (toon-shader decision executed; no outlines).
- <span class="pill todo">Todo</span> Real models where placeholders remain: relic/artifact scenes (the catalogue `scene` key is ready), item cubes, the 3 modeler creatures textured, spellbook art.
- <span class="pill todo">Todo</span> Goblin wizard extra animations (idle, hat-tip, ta-dah).
- <span class="pill todo">Todo</span> The [3D backlog](backlog-3d.md) burn-down (lantern/flag-post, night lighting final).

### 0.18 — Ship prep <span class="pill wip">WIP</span>

*Everything Steam needs for a keys-based beta.* (App ID **4987130** exists — see [Steam Store Assets](steam-store-assets.md).)

- <span class="pill todo">Todo</span> **SteamPipe build pipeline**: automated export (Windows first) → depot upload → a **private branch**; versioned builds land on Steam instead of hand-shared zips.
- <span class="pill wip">WIP</span> Export presets verified per target (Windows preset fully filled in 2026-07-24: version fields, icon, embedded PCK, shader baker); crash logging exists; the in-game feedback note (C) exists; still want a visible Discord/report link.
- <span class="pill wip">WIP</span> **RBL**: working tree is already clean (2026-07-19); the **git-history purge** stays the hard gate for anything *public or source* — a keys beta ships binaries only, so the purge may trail 0.20, but it must land before any public phase. Repo stays private regardless.
- <span class="pill idea">Idea</span> Store assets (capsules, screenshots) are **not** required while the page is hidden — deferred until a public phase is scheduled.

### 0.19 — Beta candidate <span class="pill todo">Todo</span>

- <span class="pill todo">Todo</span> **Feature freeze** for the series; only fixes and tuning land.
- <span class="pill todo">Todo</span> Bug bash across the [Known bug watch](backlog.md) + a full balance pass driven by BalanceLogger data.
- <span class="pill todo">Todo</span> Candidate builds soak with the inner circle; the 0.20 checklist below gets ticked.

### 0.20 — Steam private beta 🎉 <span class="pill todo">Todo</span>

*The testing phase begins.*

- <span class="pill todo">Todo</span> Builds distributed via **Steam keys / private branch** to a hand-picked circle; store page stays hidden.
- <span class="pill todo">Todo</span> Entry checklist: save/profiles solid across updates (save-format versions honoured), meta loop rewarding, tutorial teaches, wave 10+ smooth, zero known crashes, feedback channel instrumented.
- <span class="pill idea">Idea</span> What comes after 0.20 (public demo? Playtest? Early Access?) is a decision for during the beta — parked in [Decisions](decisions.md).

## Bigger bets (not scheduled in 0.1x)

Parked, not rejected — pull one in only by consciously displacing milestone content:

- **Buildable defenses & traps** (buy-then-throw into place) — the trap-family artifacts are the seed.
- **Flying enemies** (moth, thief flyer) — parked on aimed-throw AA feel.
- **Menu tower wears your saved artifacts** (depends on the save system + placed-artifact rework).
- Water element / new fluids, biomes, skins, mix-2-specialization wizards, cutscenes — the idea pile.

!!! note
    This roadmap is the *direction*; the [Backlog](backlog.md) is the *inventory*. When a milestone ships, move its leftovers forward explicitly and tag the release — the fastest way to see what "now" means.
