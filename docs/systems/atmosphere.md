# Atmosphere & VFX

The big visual/mood systems that make the siege feel alive. These were a major push and are largely built.

## Fire

Stylized **fire** that erodes with a tiled noise texture (which killed earlier square artifacts). Spider **body-fire** is done: a packed `spider_fire.tscn` with an **external baked process material** (`spider_fire_process.tres`, 512 surface points, `local_coords = true`). The bake sticks because the material is external/shared; `enemy.gd` just toggles emitting.

!!! note "Lesson: don't world-coord the spider fire"
    The spider fire uses local coordinates so the flame rides the moving body correctly. Earlier world-coord attempts left the fire behind the enemy.

## Oil wall-fluid system

An **oil / wall-fluid system** (`FluidField`) that creeps, stains, and pools down the tower. It's the substrate for the [oil ultimate](ultimates.md) and can be **ignited by fire** into a fire wave. Oil is currently restored to a tar look.

## Day/night cycle

A **day/night cycle**: moon, twilight, torches, fireflies, depth fog, backdrop. (SDFGI was evaluated and **dropped**.) An autoload guard keeps the fluid/fog/lights/day-night systems **dormant in menus**: they only run when the current scene is an actual `Node3D` level (see the [Start Menu note](../project/decisions.md)).

## Blood & dissolve

- A **300-bubble blood** effect for kills.
- A **reusable dissolve shader** used across effects (corpses, despawns).

## Post-processing

The look is finished with **AGX tonemapping**, tuned glow that only catches HDR-bright emissives (no global haze), SSAO, contrast/saturation lift, and a warm sun. A single-pass `screen_fx.gdshader` CanvasLayer (below the HUDs) does vignette, structured so chromatic aberration / grain can be added in the same pass rather than stacking overlays.

## Related

- [Art Direction](../tech/art-direction.md): the overall look these serve.
- [Ultimates & Spellbook](ultimates.md): oil/frost/slime built on these systems.
- [Combat, Status & Feedback](combat.md): where fire/poison/etc. get applied.
