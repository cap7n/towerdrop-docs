# Atmosphere & VFX

The big visual/mood systems that make the siege feel alive. These were a major push and are largely built.

## Fire

Stylized **fire** that erodes with a tiled noise texture (which killed earlier square artifacts). Spider **body-fire** is done: a packed `spider_fire.tscn` with an **external baked process material** (`spider_fire_process.tres`, 512 surface points, `local_coords = true`). The bake sticks because the material is external/shared; `enemy.gd` just toggles emitting. (See [Technical Solutions](../tech/solutions.md) for why it uses local coordinates.)

## Oil wall-fluid system

An **oil / wall-fluid system** (`FluidField`) that creeps, stains, and pools down the tower. It's the substrate for the [oil ultimate](ultimates.md) and can be **ignited by fire** into a fire wave. Oil is currently restored to a tar look.

## Day/night cycle

A **day/night cycle**: moon, twilight, torches, fireflies, depth fog, backdrop. (SDFGI was evaluated and **dropped**.) An autoload guard keeps the fluid/fog/lights/day-night systems **dormant in menus**: they only run when the current scene is an actual `Node3D` level (see the [Start Menu note](../project/decisions.md)). The menu has its **own self-contained mini cycle** (`menu_atmosphere.gd` on the MenuWorld root — its exports are the menu's tuning surface) which copies the level's fog numbers at startup (`use_level_fog`).

**The sky is the Binbun `TD_Sky_01`** (`res://assets/BinbunSky/skies/`, our tunable copy — colours/clouds/parallax/stars on `TD_sky_material_01.tres`). Its shader blends day/sunset/night + stars off the **sun's direction**, so both cycles drive it just by moving their sun. Hard-won wiring rules: sky `process_mode` must be **Realtime** (Quality freezes the radiance → bright-all-night), the sun's energy is **floored at 0.05** at night (at exactly 0 the engine drops it from the sky pass and the blend freezes), and the moon is `SKY_MODE_LIGHT_ONLY` so it never hijacks the blend.

## Environment ownership (the clash rule)

**The scene owns the Environment + Sky; code only animates at runtime and never force-assigns.** The environment lives in a saved resource — **`Levels/Scenes/TD_Level_Environment.tres`**, assigned to the WorldEnvironment of **both level_1 and menu_world** — tuned in the inspector, where edits persist. Runtime writes (day/night ambient, GroundFog fog) never save to disk, so the two sides can't stomp each other. Fog *values* are the exception: they live in code (`Autoloads/ground_fog.gd` vars — depth fog + the world-edge backdrop cylinder), with `fog_aerial 0.85` so fog takes the **live sky colour** and `fog_sky_affect` kept low (~0.08; high values grey out the sky's clouds/stars). Metals: `reflected_light_source = disabled` — sky reflections washed the steel bright.

## The graded look (recovered recipe)

The colour depth comes from the Environment's **Adjustments block** — losing it = instantly washed-out. The recipe (recovered from git after it was lost in an env rebuild): **AgX tonemap + exposure 1.05**, **adjustment contrast 1.15 + saturation 1.55**, **SSAO 1.6**, **glow 0.7 / HDR threshold 1.1**. All baked into `TD_Level_Environment.tres`.

## Blood & dissolve

- A **300-bubble blood** effect for kills.
- A **reusable dissolve shader** used across effects (corpses, despawns).

## Post-processing

The tonemap/glow/SSAO/adjustments now live in `TD_Level_Environment.tres` (see the graded-look recipe above). On top of that, a single-pass `screen_fx.gdshader` CanvasLayer (below the HUDs) does vignette, structured so chromatic aberration / grain can be added in the same pass rather than stacking overlays.

## Effect library (CC0)

Beyond the hand-built systems above, the project has two **Binbun Effects Collections** (Vol.1 + Vol.2, all CC0, native Godot) staged at `Tower-Cart-Game-Claude\AssetPacks\`, with the used kits adopted into `res://assets/` (beams, fire, poison, impacts, electric, StatusFX, skies — plus our `TD_`-prefixed custom effects built on their pieces). The **[VFX Checklist](../project/vfx-checklist.md)** maps every one to the game moment it fills (need vs have), and the **[VFX Pack Catalog](../project/vfx-pack-catalog.md)** documents what every pack contains.

## Related

- [VFX Checklist](../project/vfx-checklist.md): what we need vs have, and which pack effect fills each gap.
- [Art Direction](../tech/art-direction.md): the overall look these serve.
- [Ultimates & Spellbook](ultimates.md): oil/frost/slime built on these systems.
- [Combat, Status & Feedback](combat.md): where fire/poison/etc. get applied.
