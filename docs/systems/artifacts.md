# Artifacts & Relics

**Artifacts are PHYSICAL objects** (since 2026-07-19): a relic does **nothing while it sits in the bag** — it only works while **mounted on the tower**. This is the foundation of the planned **traps / hang-from-the-tower family**: special artifacts hung on the wall that run passive effects.

## The loop

1. **Get artifacts**: the main faucet is **the DRAFT (2026-07-21)** — after **every wave**, three artifact cards slam into the centre of the screen (the reusable `CardPick` chooser, time crushed to 5% while you decide) and the player **picks 1**, which lands in the bag. Wave 1's offer always includes the Coin Maker (the economy starter); duplicates across waves are welcome, since copies stack additively and the game leans hoard-of-many over few-but-powerful. Enemies also keep a small drop chance on kill (4% placeholder), and the F4 debug panel can grant randoms for testing. (The old fixed wave-1 Coin Maker grant is gone.)
2. **The bag** (right-edge ✦ tab, below the Spellbook): a **3×3 slot grid**. Owned-and-unplaced artifacts show as colour-tinted icons with a ×N badge for stacks; hover an icon for a tooltip (name, effect, bag/mounted counts); **click an icon to start placing it**. A mounted copy *leaves* the bag — the grid shows only what you can still place.
3. **Placement**: a ghost preview snaps to valid surfaces; left-click mounts, right-click/Esc cancels, and with more copies in the bag placement **continues automatically** until you run dry or cancel. A **double-click banks the held copy** back to the bag.
4. **Surfaces** — two flags per artifact: **`hang`** (the side wall: any upright surface in the wall ring, bricks included) and **`top`** (upward faces on the tower's upper half: the floor, the rim the wizards stand on, the side bits).
5. **The Mover tool** (button beside the Repair tray, debug looks for now): click a mounted artifact to pick it back up and re-place it; a quick second click sends it to the bag. On-place bonuses revert correctly on pickup.
6. **Wall risk**: an artifact hung on the wall links to the brick it hangs on (the same coverage rule the armor uses). If that brick **breaks**, the artifact is knocked loose and returns to the bag. Top mounts are safe — so wall placement is close to the action but destructible, the top is passive but protected.

## Effects: only while mounted, only while a wave is live

Tick effects (the Coin Maker) run **only during telegraph/assault** — rest, grace and menus pause the timer (no farming the downtime). Stat bonuses aggregate via `ArtifactInventory.bonus(type)` counting **mounted copies only**, and stack **additively from base** (two +5% = +10%, not compounding — decided 2026-07-19).

**The full list of artifacts and their effects lives on the [Artifact Catalogue](artifact-catalogue.md) page.** In short: six passive stat bonuses (Coin Maker, Lucky Coin, Swift Wheels, Sharp Edge, Quick Hands, Thick Bark), a set of element turrets/emitters (Frost Beam, Fire Beam, Lure Beacon, Poison Vent, Shock Coil, Dart Spitter, Oil Dripper), and one defensive relic (Crest Shield, +10 armour HP to the brick it hangs on).

The passives use the placeholder **0.5³ cube** tinted the artifact's colour; the turrets have their **own scenes** — the first users of the per-artifact `scene` key. Each catalogue entry can name its **own scene** (`scene` key; root script extends `TowerArtifact`, a `mount_offset` export seats the mesh on the surface) — that's the path to real relic models.

## Tech notes

- `Items/Scenes/tower_artifact.tscn` + `TowerArtifact` (`Items/Scripts/tower_artifact.gd`); pick colliders live on collision layer 8 **ArtifactPick**.
- **It IS a real hierarchy, not one-off scripts:** `TowerArtifact` is the base (mount/pickup, `_wave_live()`, `_visual`, `mount_offset`) and four behaviour subclasses `extends TowerArtifact` — `artifact_laser` (beams: Frost + Fire), `artifact_emitter` (proximity: Lure / Poison / Electric), `artifact_darts` (Dart Spitter), `artifact_oiler` (Oil Dripper), plus `artifact_shield` for Crest Shield. One script serves every element of its family; the catalogue's `scene` key picks the script and `props` push the per-element values in. **So a behaviour change lands on the whole family at once** — e.g. the beam kill-cooldown applies to Frost and Fire both, and per-element differences belong in the catalogue `props`, not in new scripts.
- **Drop supply = guaranteed draft + a RAMPING pity chance** (2026-07-25, Yaro's design). One artifact is guaranteed per wave via the draft. On top of that, kill drops use a counter instead of a flat chance: chance starts at **0%**, every kill adds `drop_chance_step_pct` (× `DROP_STEP_BY_TYPE`, so trash spiders advance it least), it never exceeds `drop_chance_cap_pct`, and any drop **resets it to 0**. Both exports are in PERCENT so the inspector number matches how the design is spoken. The counter rides in the run checkpoint.
    - **Why this shape:** a flat chance clustered at both ends (two artifacts in wave 1, or a dry run). Ramping-and-resetting makes clustering impossible and guarantees a dry spell ends.
    - **The ramp protects the early game for free.** The counter starts at zero and wave 1 is only ~6 kills, so even a 30× faster ramp averages 0.06 drops in wave 1. Early-wave scarcity costs nothing in tuning.
    - **The CAP is the supply dial, not the ramp.** Once the counter reaches the cap it sits there, so the cap sets the long-run rate: cap 1% ≈ a drop every 100 kills forever, cap 4% ≈ every 25. At step 0.1 the cap alone swings a 15-wave run between 6.2 and 15.1 drops.
    - Measured (20k sims; 8-wave run = 191 kills, 15-wave ≈ 650): `0.01/1.0` → 1.1 / 4.2 · `0.05/4.0` → 3.0 / 11.1 · **`0.1/4.0` (default)** → 4.2 / 15.1 · `0.2/4.0` → 5.4 / 18.9.
    - **Supply must outpace LOSS** — the reason the default is generous. Artifacts already get knocked loose when their brick breaks, and the plan is for them to be genuinely LOST or STOLEN (see the ideas below), so the drop rate has to refill a 9-slot bag AND a decorated tower, not just fill it once.
- `WaveManager.wave_completed` (emitted on entering REST) drives the after-wave draft (`ArtifactInventory._on_wave_completed` builds the 3-card offer and calls `CardPick.present`); `CoinSpawner.spawn_bonus_coin(pos)` is the single-coin payout.
- Catalogue + placement + bag state all live in the `ArtifactInventory` autoload; the run's collected state still resets on game over, but artifact **unlocks** persist per profile and are LIVE (2026-07-25): the after-wave draft, kill drops and the debug grant all filter through `Achievements.is_artifact_unlocked()` — an artifact is locked iff an achievement rewards it and isn't earned yet (Sharp Edge + Thick Bark are the first two gated ids). Playtime + shard unlock routes still to come; see Meta-progression in the [Backlog](../project/backlog.md).

## Real models (replacing the placeholder cubes)

Recipe, for the next one: export the `.blend` with `export_creature.py` (in the artifacts folder's `My blender models/`) → `res://Blender/<Name>.glb`, copy `artifact_shield.tscn` as the template (TowerArtifact root + model under `Visual` + `PickBody` on layer 128), then add a `"scene": preload(...)` key to the catalogue entry. No other code changes.

⚠️ **Two gotchas:**

- `export_creature.py` **grounds** models (origin at the bounding-box bottom) but the placement code positions an artifact by its **centre** — so shift the model down by half its scaled height inside `Visual`, or it floats/sinks on every mount.
- Godot needs **two `--import` passes** for a new GLB: the first registers the `.glb`, and a scene referencing it fails to load until the second. A one-off "Failed loading resource" on the first pass is expected, not a broken export.

| Artifact | Model | Notes |
|---|---|---|
| Crest Shield | `Blender/Shield.glb` | Thomas's crest shield |
| Coin Maker | `Blender/ARTI_CoinMaker.glb` | 2026-07-25; natural 0.707 × 0.831 × 0.716, scaled 0.65 |

The other seven still use the tinted placeholder cube.

## Losing artifacts (designed 2026-07-25, not built)

Artifacts are meant to be **at risk**, not a one-way collection — that is what makes wall-vs-top placement a real decision and what the ramping drop supply is sized to feed.

- <span class="pill done">Done</span> **Knocked loose**: a wall-hung artifact links to its brick; if that brick BREAKS the artifact returns to the bag (top mounts are safe). Built 2026-07-19.
- <span class="pill idea">Idea</span> **Actually LOST when the shell breaks** (Yaro 2026-07-25) — rather than returning to the bag, a knocked artifact is gone. Turns wall placement into a genuine gamble. *(Softer variant already parked: it physically DROPS as a ground pickup you can run over to recover, so losing it is a scramble rather than a deletion.)*
- <span class="pill idea">Idea</span> **Enemies STEAL artifacts** (Yaro 2026-07-25) — a thief-type climbs for a mounted relic and tries to carry it off the field; kill it before it escapes to get the artifact back. Connects to the existing enemy CARRY system and to the parked **thief flyer** idea in [Decisions](../project/decisions.md).
- Both raise the supply the economy must sustain — see the cap note above.

!!! warning "Still open"
    Trap-type effects (the actual reason the family exists), real relic models, a pretty mover cursor, placement slots/limits, and the artifact-shard earn source (drops? wave-clear bonus? game-over conversion?). The achievements unlock route is BUILT (2026-07-25); the playtime + shard routes are still open. See the [Backlog](../project/backlog.md).

## Related

- [Enemies](../game/enemies.md): the carry system that can feed drops.
- [The Tower](../game/tower.md): what everything mounts on.
- [Backlog](../project/backlog.md): remaining artifact work.
