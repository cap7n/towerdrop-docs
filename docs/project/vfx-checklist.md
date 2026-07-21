# VFX Checklist

What visual effects the game **needs**, what it already **has**, and which **pack effect** fills the gap.

The two **Binbun Effects Collections** (Vol.1 = 20 kits, Vol.2 = 9 packs, all **CC0**, native Godot) are staged unpacked at `Tower-Cart-Game-Claude\AssetPacks\`. Adopt a whole kit at a time into `res://assets/BinbunVFX[_Vol2]/<kit>/` so its internal paths hold, then instance the specific effect scenes.

**Legend:** <span class="pill done">Have</span> already in game · <span class="pill wip">Partial</span> exists but code-built / placeholder · <span class="pill todo">Need</span> no VFX yet · <span class="pill idea">Nice</span> optional polish.

---

## Elements (the six wizards + their items)

| Moment | Status | Pack effect to use |
|---|---|---|
| **Rock** impact on hit | <span class="pill wip">Partial</span> code-built `Vfx.spawn_impact` | `ImpactVFX/vfx_impact_*`, `vfx_hit_*`; `StatusFX/vfx_status_shatter` for a stony crack |
| **Fire** on enemy body | <span class="pill done">Have</span> hand-built spider fire | keep; `FlameFX` has cold/green/purple/void tints if other fires ever want a colour |
| **Fire** ground ignite / trail | <span class="pill wip">Partial</span> custom flame patch | `FireVFX/fire_area_*`, `fire_trail`; `FireVFX/fire_ball_*` for a thrown fire arc |
| **Electric** chain lightning | <span class="pill wip">Partial</span> code-built arcs | `ElectricFX/vfx_zap_lightning_*` (built for arc-to-arc), `vfx_impact_lightning_*` on the strike |
| **Poison** cloud / spreading death | <span class="pill wip">Partial</span> custom clouds | `PoisonVFX/poison_cloud_*`, `poison_puddle_*`, `stink_*`; `poison_bubble_*` for the slime |
| **Frost** freeze / shatter | <span class="pill wip">Partial</span> ice crust coat (custom) | `IceVFX/ice_shard_*` on freeze-shatter, `ice_mist_*` aura; `StatusFX/vfx_status_ice` |
| **Lure** beacon pull | <span class="pill todo">Need</span> | `MagicAreaVFX/pulse_area_*` or `rim_area_*` (a pulsing ground ring = the pull radius) |

## Wizard summons & casts

| Moment | Status | Pack effect to use |
|---|---|---|
| Wizard **cast** flash (conjure / fire) | <span class="pill todo">Need</span> | `ElementalMagicFX/vfx_<elem>_cast_*` (fire/electric/water/nature), `MuzzleFlashVFX` |
| Summoned **projectile** | <span class="pill todo">Need</span> | `MagicProjectiles/mprojectile_basic_*` + `_javelin_*`; `ElementalMagicFX/vfx_<elem>_projectile_*` |
| Conjured item **spawn orb** | <span class="pill idea">Nice</span> | `MagicOrbsVFX/magic_orb_*` (the parked "distortion-ball around the conjured item" idea) |

## Artifacts (the turret / hang family)

| Moment | Status | Pack effect to use |
|---|---|---|
| **Beam** artifacts (slow beam) | <span class="pill done">Have</span> BeamVFX `laser_vfx_01` wired in, recolored per element | first pack **adopted into the game** (`res://assets/BinbunVFX/beam_vfx/`); `artifact_laser.gd` drives it via `end_point` + runtime `beam_color`; six `element:beam` placeholder artifacts in the catalogue (rock/fire/electric/poison/frost/lure), all slow for now |
| **Dart Spitter** projectile | <span class="pill wip">Partial</span> | `PoisonVFX` dart + `MagicProjectiles/mprojectile_basic_*` |
| **Artifact drop / pickup** shine | <span class="pill todo">Need</span> (drops are visually mute) | `LootVFX` — rarity-tiered common→mythic, so relic tiers can read at a glance; `ground_loot_vfx_*` on the ground pickup |
| **Crest Shield** mount glint | <span class="pill idea">Nice</span> | `BattleFX/vfx_*_shield_*` (a shield-flash on hang or when it soaks a hit) |

## Combat feedback

| Moment | Status | Pack effect to use |
|---|---|---|
| Enemy **hit** spark | <span class="pill wip">Partial</span> body hit-flash only | `HitFX/vfx_hit_*`, `vfx_strike_*` (small, per-hit) |
| Big / killing blow | <span class="pill todo">Need</span> | `HitFX/vfx_big_impact_*` |
| **Explosive item** blast (explosive rock, molotov) | <span class="pill todo">Need</span> | `ExplosionFX/vfx_burst_*`, `vfx_ground_explosion_*`; `ImpactVFX/vfx_explosion_*` |
| **Status active** aura on enemy (poisoned / frozen / burning) | <span class="pill todo">Need</span> | `StatusFX/vfx_status_poison` · `_ice` · `_flame` · `_rot` · `_sleep` — small loops, plug into StatusDB |
| Death **smoke / dust** | <span class="pill idea">Nice</span> | `SmokeVFX/smoke_thin_*`, `smoke_big_*` |

## Ultimates

| Moment | Status | Pack effect to use |
|---|---|---|
| Oil / Frost / Slime tower coats | <span class="pill done">Have</span> custom fluid system | keep |
| **Meteor shower** (planned rock ult) | <span class="pill todo">Need</span> | `FireVFX/fire_ball_*` falling + `ExplosionFX/vfx_ground_explosion_*` on landing |
| Cast **telegraph** on ultimate fire | <span class="pill idea">Nice</span> | `MagicAreaVFX/pillar_area_*`, `ElementalMagicFX/vfx_<elem>_area_*` |

## World & waves

| Moment | Status | Pack effect to use |
|---|---|---|
| Wave **signal flare** telegraph | <span class="pill done">Have</span> code-built firework | keep (externalize to a scene is a separate 3D-backlog task) |
| Enemy **spawn** from the sector | <span class="pill idea">Nice</span> | `PortalVFX/portal_vfx_*` — enemies emerging from a portal would sell the directional-wave sector |

## UI, menus & meta

| Moment | Status | Pack effect to use |
|---|---|---|
| **3-card draft / element pick** juice | <span class="pill todo">Need</span> | `CardFX` — purpose-built for card reveals; pairs with the revamp's slam-in cards |
| Menu → game **screen wipe** | <span class="pill todo">Need</span> | `Transitions` kit |
| Shop / spellbook **panel treatment** | <span class="pill idea">Nice</span> | `GlassUI`, `FrostedGlassFX`, `HologramFX` |
| Alternate **skyboxes** | <span class="pill idea">Nice</span> | `Skies` kit (compare against the current day/night sky) |

## Not for us (leave in the box)

- **UltimateToonShader** — outline-based toon look; conflicts with the [no-outlines rule](../tech/art-direction.md).
- **Grass**, **Water** — grass is already Quaternius (settled after a whole saga); no water body in the game yet.
- **DarkMagicFX** (void/evil), **MagicOrbs huge/nuke** — no dark/void theme; hold unless a boss or corruption enemy appears.

---

!!! note "How to pull one in"
    Copy the whole kit folder into `res://assets/…`, let Godot import, open the kit's `*_full` / `*_scene` demo to preview every effect, then instance the specific `vfx_*.tscn` you want as a child of the thing it decorates (or spawn it via the `Vfx` autoload for one-shots). Start with the ones Cap7n called out: **BeamVFX → Laser Lens**, **FireVFX → fire item**, **StatusFX → status auras**, **LootVFX → artifact drops**.
