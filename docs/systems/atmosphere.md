# Atmosphere & VFX

The big visual/mood systems that make the siege feel alive. These were a major push and are largely built.

## Fire

Stylized **fire** that erodes with a tiled noise texture (which killed earlier square artifacts). Spider **body-fire** is done: a packed `spider_fire.tscn` with an **external baked process material** (`spider_fire_process.tres`, 512 surface points, `local_coords = true`). The bake sticks because the material is external/shared; `enemy.gd` just toggles emitting. (See [Technical Solutions](../tech/solutions.md) for why it uses local coordinates.)

## Oil wall-fluid system

An **oil / wall-fluid system** (`FluidField`) that creeps, stains, and pools down the tower. It's the substrate for the [oil ultimate](ultimates.md) and can be **ignited by fire** into a fire wave. Oil is currently restored to a tar look.

## Day/night cycle

A **day/night cycle**: moon, twilight, torches, fireflies, depth fog, backdrop. (SDFGI was evaluated and **dropped**.) An autoload guard keeps the fluid/fog/lights/day-night systems **dormant in menus**: they only run when the current scene is an actual `Node3D` level (see the [Start Menu note](../project/decisions.md)).

## Blood & dissolve

- A **300-bubble blood** effect for kills.
- A **reusable dissolve shader** used across effects (corpses, despawns).

## Post-processing

The look is finished with **AGX tonemapping**, tuned glow that only catches HDR-bright emissives (no global haze), SSAO, contrast/saturation lift, and a warm sun. A single-pass `screen_fx.gdshader` CanvasLayer (below the HUDs) does vignette, structured so chromatic aberration / grain can be added in the same pass rather than stacking overlays.

## Effect library (CC0)

Beyond the hand-built systems above, the project has two **Binbun Effects Collections** (Vol.1 + Vol.2, all CC0, native Godot) staged at `Tower-Cart-Game-Claude\AssetPacks\` — beams, fire, electric, poison, ice, impacts, status auras, loot shines, explosions, portals, card/UI FX. The **[VFX Checklist](../project/vfx-checklist.md)** maps every one to the game moment it fills (need vs have).

## Related

- [VFX Checklist](../project/vfx-checklist.md): what we need vs have, and which pack effect fills each gap.
- [Art Direction](../tech/art-direction.md): the overall look these serve.
- [Ultimates & Spellbook](ultimates.md): oil/frost/slime built on these systems.
- [Combat, Status & Feedback](combat.md): where fire/poison/etc. get applied.
