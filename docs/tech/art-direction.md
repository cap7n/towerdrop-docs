# Art Direction

## The look

**Stylized, leaning faceted-flat / soft-matte.** Strong albedo, restrained specular, the occasional rim highlight on important props. The warm AGX + glow + saturation post-processing (see [Atmosphere & VFX](../systems/atmosphere.md)) is part of the look, not a filter on top of it.

## The hard rule: no outlines

**No cartoon outlines.** They've been tried and disliked. Don't reach for an outline shader to "make it pop" — the pop should come from form, albedo, lighting, and rim highlights.

- Parked toon-shader experiments live in `VFX/Shaders/toon*.gdshader`. They are **not** the committed direction — kept for reference only.

## Placeholders vs. final

The current nature/prop assets (grass, rocks, trees) are **placeholders**. The **full art pass waits for real models** — don't over-invest in tuning placeholder materials. The exception is targeted feel work (e.g. grass wind, coin shader) that pays off regardless of final assets.

## Imported art

Curated 3D/VFX assets live under `res://Imported/` — see the [Asset Pipeline](asset-pipeline.md) for the Roll-Call imports, the new modeler tower, and import conventions.

## Related

- [Design Pillars](../pillars.md) — where "no outlines" and "physical feedback" come from.
- [Atmosphere & VFX](../systems/atmosphere.md) — the post-processing and effects that carry the mood.
- [Asset Pipeline](asset-pipeline.md) — how art gets into the game.
