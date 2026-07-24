# Roadmap & Priorities

The through-line. For the granular task list, see the [Backlog](backlog.md).

**The prototype phase is over.** The 0.0x line (v0.014 → v0.017) proved the game; from here the project runs on a **versioned milestone roadmap**: the **0.1x alpha series**, ending at **0.20 = the Steam private-beta phase** (builds distributed via Steam keys / a private branch to a hand-picked circle; the store page can stay hidden). Dates are deliberately absent — a milestone ships when its theme is playable in a tester build.

## Versioning rules (start at 0.10)

Decided 2026-07-24 (Yaro):

- **Every shipped tester build gets a version.** Bump the version in the Godot project (`config/version` in project.godot) **and** the export preset fields (`application/product_version` + `application/file_version` in `export_presets.cfg` — both currently empty; first fill = 0.10), so the exe itself carries the number. The menu version label and a `CHANGELOG.txt` entry ride along as before.
- **Git tag per release**: tag the release commit `v0.10`, `v0.11`, … in the game repo.
- **Engine pinned per release**: currently **Godot 4.7** (`config/features`). Engine upgrades happen only at a version boundary and get recorded here — never mid-milestone.
- **Numbers are themes, not dates.** Content can slip between milestones; the numbering never reorders. A theme that lands small may merge into its neighbour (we'd rather skip a number than ship a hollow one).

## The 0.1x series — alpha, milestone by milestone

Each milestone names its theme, its goal, and the backlog items that define "done". Within any milestone the [pillars](../pillars.md) tie-breaker applies: **base-game fun and feel first**.

### 0.10 — A game you can put down

*The "left prototype" build: runs persist, the build is clean.*

- **Save system v1, per profile** — the full plan lives in the [Backlog](backlog.md) (Bigger features): REST-checkpoint saves (only when the field is clear — no enemy presence), `SaveSystem` autoload, named profile slots (`user://profiles/`), menu **Continue**, game-over deletes the run file. The Profiles button stops being a stub.
- Clean **restart after game-over** without relaunching (also chases the build-only restart crash — see Known bug watch).
- Build hygiene leftovers: remaining raw cheat keys folded into the F4 sandbox; test scenes confirmed excluded from exports.
- First build stamped + tagged per the versioning rules above.

### 0.11 — The meta loop

*Losing a run still moves you forward.*

- **Meta layer** (`meta.json`, survives game over): playtime, achievements tracker v1, artifact shards. See the Meta-progression section of the [Backlog](backlog.md).
- **Artifact meta-unlocks live**: the draft draws only from the profile's unlocked pool; unlock routes = playtime / achievements / shards.
- **Decide the shard source** (elite drops? wave-clear bonus? game-over conversion) — the one open design call blocking this milestone.
- Profiles screen shows meta progress (playtime, shards, unlock count).

### 0.12 — Every system does what it says

*No dead items, no placebo nodes.*

- **Item → effect coverage**: every buyable/usable item does something.
- The **~20 skill-tree nodes with no effect code** (wet_conductor, wildfire, ricochet, boulder, shatter, …) — element-by-element sessions.
- **Ultimates completed**: FireWave / Arc Storm / Black-hole Lure effect code; spikes folded in as a Rock spell; per-spell costs; wizard-buff inheritance decision executed.
- **Elemental combos**: Electric + Oil (arc conductor) — the next nearly-free one.
- Per-element enemy resistance/weakness system promoted from quick-test to real.

### 0.13 — The horde

*The enemy roster and the long game.*

- **Enemy component system migration** finished (A/B Spider_v2, port Snail, migrate subclasses, delete the legacy inline path).
- New enemies wired into waves: **termite, bombardier, carrier**; centipede finished (segment colliders, mid-split); **turtle / fly / worm** designed and given behaviour.
- **Elemental spiders** (fire ~W3, trickle from ~W5) and the "only THIS element penetrates" layer.
- **Waves beatable to ≥10** tuning + late-game generator tuning (the C4 marathon list: frost buff, element cost rebalance, roller aim).
- Revamp Track 2 leftover: **make early choices HIT** (cheap dramatic first tiers, re-tune waves 1–5).

### 0.14 — Onboarding

*A stranger can learn the game without us in the room.* (Deliberately after 0.12/0.13 — the [Revamp](revamp.md) parked the tutorial until the systems it teaches stop moving.)

- **Tutorial mode finished**: the full Track 1 curriculum (Blocks 0–5, 10–15 min, no game-over pressure), completion flag, first-launch nudge.
- **Controls reference** card + quick single-screen tutorial.
- Revamp Track 2 leftovers: **telegraph upgrades loudly** (gold-nudge, glowing shop entries) and the **system reveal order** pass.
- Tooltip / description copy fill + spellcheck.

### 0.15 — Runs smooth

*Wave 10+ doesn't chug on a tester's machine.*

- **Perf Phase 0** quick wins (shadows, alloc flood, ragdoll caps — see [Performance](../tech/performance.md)); sustained-cost profiling (double tower coats / FluidField).
- **Restart-run crash** closed out (crash logs are already being collected).
- Game file size + load-time pass.

### 0.16 — Looks & sounds (polish I)

- **Audio completion**: ultimate casts, item impacts, enemy deaths, wave-warning tie-in; music intensity ramps with waves (from the idea pile).
- **HUD readability pass** (wave / HP / gold / loadout) + menu fog tuning.
- VFX debts: magical **aim-line** treatment, **per-element beam identities**, grass pop-back smoothing.

### 0.17 — Art pass (polish II)

- **Stylized look settled** (toon-shader decision executed; no outlines).
- Real models where placeholders remain: relic/artifact scenes (the catalogue `scene` key is ready), item cubes, the 3 modeler creatures textured, spellbook art.
- Goblin wizard extra animations (idle, hat-tip, ta-dah).
- The [3D backlog](backlog-3d.md) burn-down (lantern/flag-post, night lighting final).

### 0.18 — Ship prep

*Everything Steam needs for a keys-based beta.* (App ID **4987130** exists — see [Steam Store Assets](steam-store-assets.md).)

- **SteamPipe build pipeline**: automated export (Windows first) → depot upload → a **private branch**; versioned builds land on Steam instead of hand-shared zips.
- Export presets verified per target; crash logging + the in-game feedback note (C) confirmed working in Steam builds; a visible Discord/report link.
- **RBL**: working tree is already clean (2026-07-19); the **git-history purge** stays the hard gate for anything *public or source* — a keys beta ships binaries only, so the purge may trail 0.20, but it must land before any public phase. Repo stays private regardless.
- Store assets (capsules, screenshots) are **not** required while the page is hidden — deferred until a public phase is scheduled.

### 0.19 — Beta candidate

- **Feature freeze** for the series; only fixes and tuning land.
- Bug bash across the [Known bug watch](backlog.md) + a full balance pass driven by BalanceLogger data.
- Candidate builds soak with the inner circle; the 0.20 checklist below gets ticked.

### 0.20 — Steam private beta 🎉

*The testing phase begins.*

- Builds distributed via **Steam keys / private branch** to a hand-picked circle; store page stays hidden.
- Entry checklist: save/profiles solid across updates (save-format versions honoured), meta loop rewarding, tutorial teaches, wave 10+ smooth, zero known crashes, feedback channel instrumented.
- What comes after 0.20 (public demo? Playtest? Early Access?) is a decision for during the beta — parked in [Decisions](decisions.md).

## Bigger bets (not scheduled in 0.1x)

Parked, not rejected — pull one in only by consciously displacing milestone content:

- **Buildable defenses & traps** (buy-then-throw into place) — the trap-family artifacts are the seed.
- **Flying enemies** (moth, thief flyer) — parked on aimed-throw AA feel.
- **Menu tower wears your saved artifacts** (depends on the save system + placed-artifact rework).
- Water element / new fluids, biomes, skins, mix-2-specialization wizards, cutscenes — the idea pile.

!!! note
    This roadmap is the *direction*; the [Backlog](backlog.md) is the *inventory*. When a milestone ships, move its leftovers forward explicitly and tag the release — the fastest way to see what "now" means.
