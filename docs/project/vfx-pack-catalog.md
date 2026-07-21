# VFX Pack Catalog

A per-pack reference for every **Binbun** asset pack across the two bundles we're looking at, so we can pick the right kit for a job without re-crawling itch. All packs are native Godot and (with three noted exceptions) **CC0** ship-safe. Cross-reference with the [VFX Checklist](vfx-checklist.md), which maps these to specific game moments.

**The two bundles**

- **Godot VFX Bundle (700+ assets)** [`itch.io/s/178769`](https://itch.io/s/178769/godot-vfx-bundle-700-assets) — **$35** (reg. $106.88, 67% off). 27 packs: all the core VFX kits + shaders + the Retro material packs + 3D model packs + Effects Collection Vol.1.
- **Binbun Effects Bundle Vol.2** [`itch.io/s/183825`](https://itch.io/s/183825/binbun-effects-bundle-vol-2) — **$24.99** (reg. $54.94, 54% off). 16 packs: the six shared material/3D packs + nine newer FX kits + Effects Collection Vol.2.

!!! warning "Licensing split"
    Almost everything is **CC0** (no attribution). The **three Retro material packs (RetroGround, RetroWood, RetroUrban) are CC BY 4.0** — usable commercially but **require crediting Binbun3D**. If we ever ship those, they go in the credits. Everything else needs no attribution.

**Legend:** <span class="pill done">Wired</span> already in the game · <span class="pill todo">Gap-filler</span> maps to a checklist "Need" · <span class="pill idea">Maybe</span> situational · <span class="pill risk">Skip</span> out of scope for us.

Every FX kit shares the same Binbun tool-script framework: exposed `primary/secondary/tertiary_color`, emission, proximity fade, speed, and transparency modes (Smooth / Dithered / Cut / Hybrid) — so the "copy kit into `res://assets/`, instance the scene, recolor per element" pattern applies to all of them.

---

## Elemental VFX

### Fire VFX <span class="pill done">Wired</span>
30 stylized fire effects. We use `fire_area_01` on the burning ground patch.

- 10 Flame effects (varying sizes/shapes) · 6 Fireball effects · 4 Force Field effects · 8 Ground Area effects · Candle Flame · Trail Flame
- **30 effects.** CC0. Tool scripts for emission + one-shot.

### Flame FX (Vol.2) <span class="pill idea">Maybe</span>
34 stylized flame presets — campfires, torches, projectiles, status. Overlaps Fire VFX but "flows naturally when moved," so better for a flame that travels (a burning enemy, a torch on the cart).

- 12 flame styles, each with an overlay material; size variations
- **34 presets.** CC0. Colors, wobble, edge softness; noise-driven flow.

### Ice VFX <span class="pill todo">Gap-filler</span>
20 ice/freezing effects — maps to the frost freeze/shatter checklist row.

- 4 Ice Areas · 4 Ice Projectiles · 4 Ice Balls · 4 Small Sharp Ice Clouds · 4 Cold Mist Clouds
- **20 variants.** CC0. Full recolor + transparency modes.

### Poison VFX <span class="pill todo">Gap-filler</span>
24 poison/radioactive effects — the poison cloud / spreading-death rows.

- 4 Bubbly ooze puddles · 4 Poison ripple areas · 4 Poison clouds · 8 Poison smoke/smell · 4 Poison pops
- **24 effects.** CC0.

### Electric FX (Vol.2) <span class="pill todo">Gap-filler</span>
18 electric effects — the chain-lightning / electric-impact rows.

- Lightning zap effects · Electric ball effects · Electric impact effects (page lists categories, not per-preset names)
- **18 presets.** CC0. Shape via noise frequency/amplitude, lightning height.

### Elemental Magic FX (Vol.2) <span class="pill idea">Maybe</span>
24 presets across fire/water/electricity/nature, split into projectile / area / casting — a one-stop kit for wizard casts + summoned projectiles.

- 8 projectile · 8 area · 8 casting (2 variations per element)
- **24 presets.** CC0. Element-specific params (wave, spiral, tail length, area radius).

---

## Impact, Combat & Explosions

### Impact VFX <span class="pill todo">Gap-filler</span>
20 impact/explosion effects — rock-impact and killing-blow rows.

- 3 Big Explosions · 3 Small Explosions · 6 Ground Impacts · 8 Hit effects
- **20 effects.** CC0.

### Hit FX (Vol.2) <span class="pill todo">Gap-filler</span>
28 hit effects from snappy taps to magic impacts — the per-hit spark row.

- Basic → magic-style impacts; flare/streak/explosion shaders; size + buildup variation
- **28 presets** (6 free). CC0. Godot 4.4–4.7.

### Explosion FX (Vol.2) <span class="pill todo">Gap-filler</span>
16 explosion variations with built-in audio — for explosive rock / molotov / meteor landings.

- Grounded · Airborne · Mushroom cloud · smokeless impacts · charged explosions
- **16 variations.** CC0. Tool scripts for smoke-cloud props + audio.

### Battle FX (Vol.2) <span class="pill idea">Maybe</span>
42 melee/shield presets. The shield sub-kit could be the Crest Shield mount-glint.

- 12 Shield presets (2 types) · 6 Flying slash · 6 Swing · 6 Claw · 6 Charge
- **42 presets.** CC0. Audio integration.

### Muzzle Flash VFX <span class="pill idea">Maybe</span>
24 muzzle flashes. No guns in TowerDrop, but a small flash sells a wizard cast or dart-spitter shot.

- 6 each of Medium / Small / Very small / Big
- **24 effects.** CC0.

### Smoke VFX <span class="pill idea">Maybe</span>
24 smoke presets (smooth + toon) — death dust, chimney/torch ambiance.

- 8 each Big / Medium / Thin, smooth & toon variants
- **24 presets.** CC0. Density, edge style, shading, shadows via Smoke Controller.

---

## Magic, Casting & Projectiles

### Magic Area VFX <span class="pill todo">Gap-filler</span>
30 ground-glow area effects — the Lure beacon pull-radius, ultimate telegraphs.

- 5 each: Small · Misty Glow · Ripple · Pulse · Tall Pillar · Lift
- **30 effects.** CC0.

### Magic Orb VFX <span class="pill idea">Maybe</span>
30 orb effects — the parked "distortion ball around the conjured item" idea.

- 10 Small · 10 Misty Spiral · 5 Big Glowing Rim · 5 Big Burst
- **30 effects.** CC0. Adjustable mesh resolution.

### Magic Projectiles VFX <span class="pill todo">Gap-filler</span>
12 projectile effects — summoned projectiles, dart-spitter rounds.

- 4 Basic · 4 Wavy · 4 Sharp Javelin
- **12 effects.** CC0. LOD optimization.

### Portal VFX <span class="pill done">Wired</span>
24 preset portals with stencil-buffer support. We use `simple_portal_vfx` as the recolored conjure ring; could also mark enemy spawn sectors.

- Circular / Oval / Rectangular, 24 texture×shape presets
- **24 presets.** CC0. Parallax depth, lens warp, open/close animation.

### Beam VFX <span class="pill done">Wired</span>
16 preset beams. We use `laser_vfx_01` on the element:beam artifacts, anchored to the target via `end_point`.

- LaserVFX_01–16 (1 base beam in free)
- **16 presets.** CC0. Radius/flare/edge, pulse, swappable start/mid/end audio, **endpoint snapping to a target node**.

### Dark Magic FX (Vol.2) <span class="pill risk">Skip</span>
21 void/evil effects. No dark theme in TowerDrop — hold unless a corruption boss appears.

- 6 Projectile · 6 Orb · 6 Area · 3 Vortex
- **21 presets.** CC0.

---

## Pickups & Status

### Loot VFX <span class="pill todo">Gap-filler</span>
12 rarity-tiered drop effects — artifact drop/pickup shine, tiers readable at a glance.

- Common / Uncommon / Rare / Epic / Legendary / Mythic, each grounded + floating
- **12 variants.** CC0. Custom icon integration, light-beam tweaks.

### Status FX (Vol.2) <span class="pill done">Wired</span>
36 status presets. We use `vfx_status_glow` as the enemy immunity/resistance aura; the poison/ice/flame variants are the "status active" gap.

- 18 preset effects + 18 overlay materials (auras, light streaks, textured particles)
- **36 presets.** CC0. Color-transition curve, noise waviness.

---

## UI & Surface Shaders

### Modular Transitions <span class="pill todo">Gap-filler</span>
Gradient-texture transition shader — menu→game screen wipes.

- Directional / Radial / Circular; 12 pre-made transitions (full)
- **12 transitions.** CC0. Works on any 2D CanvasItem.

### Card FX <span class="pill todo">Gap-filler</span>
Parallax + holographic card shaders — the revamp's 3-card draft / element pick juice.

- Holo, diamond, foil, negative, colored-tier presets; parallax + flat versions; **24 presets**
- CC0. Pixel-art friendly.

### Hologram FX <span class="pill idea">Maybe</span>
Additive volume hologram shader — sci-fi UI panels (situational for our fantasy look).

- Scanlines · Noise · Distortion · Grain · Flicker
- CC0. Free (source) + full (preset materials).

### Fluid Glass UI <span class="pill idea">Maybe</span>
Refractive glass shader for UI nodes — shop/spellbook panel treatment.

- Edge-warp refraction, chromatic shift, blur, grain, rim light; button state presets (normal/pressed/hover)
- CC0. Driven through Godot StyleBox.

### Frosted Glass (Vol.2) <span class="pill idea">Maybe</span>
Frosted/blurred glass shader — privacy glass, ice windows, or a frosted panel.

- Refraction, gaussian + mipmap blur, tiled refraction, roughness-driven blur, aberration slider, rim light; 12 preset materials
- **12 presets.** CC0.

### Ultimate Toon Shader <span class="pill risk">Skip</span>
Cel-shading with tinted shadows + patterns. **Outline-based — conflicts with our [no-outlines rule](../tech/art-direction.md).**

- Tinted shadow steps, grayscale patterns, screen-UV mapping, rim light; 22 preset materials
- **22 presets.** CC0.

---

## World & Environment Shaders

### Skies <span class="pill done">Wired</span>
Customizable sky shader with seamless day/sunset/night. We drive it via the sun in `day_night_cycle.gd` — `stylized_cloudy_sky_01` is the current pick.

- Sun follows the scene's directional light; noise-texture clouds w/ parallax; customizable stars
- **24 preset skies** (10 realistic, 10 stylized, 4 experimental). CC0.

### Grass Shader <span class="pill risk">Skip</span>
Wind-animated grass shader. We already settled on Quaternius grass after a whole saga — hold.

- 2×2 blade atlas, noise wind, billboard option, alpha modes, gradient color; 10 presets + 10 palettes + 14 blade textures (full)
- CC0.

### Water <span class="pill risk">Skip</span>
Ocean/lake/river water shader. No water body in the game.

- World-space caustics, refraction, depth, foam; Smooth + Toon shading; 15 preset materials (full)
- CC0. Godot 4.x.

---

## Materials & 3D Models

Mostly out of genre, but here for completeness. **The three Retro packs are CC BY 4.0 (attribution required)** — the rest are CC0.

### RetroGround <span class="pill risk">Skip</span> · CC BY
104 retro PBR ground materials (128×128, full PBR maps). 20 each Dirt/Grass/Gravel/Rock, 16 Rocky Dirt, 8 Sand.

### RetroWood <span class="pill risk">Skip</span> · CC BY
50 retro/PSX wood materials — 10 wood types with plank, moldy, and 3 parquet variants each.

### RetroUrban <span class="pill risk">Skip</span> · CC BY
105 stylized urban materials (128×128) — Asphalt, Road, Concrete, Concrete Slab, Pavement.

### RPG Weapons <span class="pill risk">Skip</span> · CC0
70 (free) / 138 (pro) low-poly stylized weapons & tools. FBX/glTF/OBJ, single 1024 atlas, native Godot. Source `.blend` at paid tier.

### Food Mega Pack <span class="pill risk">Skip</span> · CC0
Low-poly food models (fruit, veg, meat, drinks, desserts, cutlery). Native Godot (Update 1.1). Wrong genre unless a food item ever appears.

### Treasure <span class="pill idea">Maybe</span> · CC0
61 (free) / 109 (pro) low-poly treasure models — **gems, coins, keys, rings, scrolls, crowns, chests.** The coins/gems/chests could serve loot, artifacts, or reward visuals. Native Godot. Source `.blend` at paid tier.

---

## Meta-collections

### Effects Collection Vol.1 <span class="pill done">Owned</span>
300+ effects re-bundling the individual kits into one pack. VFX kits: Fire, Impact, Muzzle Flash, Magic Area, Magic Orb, Magic Projectile, Poison, Ice, Smoke, Portal, Beam, Loot. Shader kits: Modular Transitions, Skies, Ultimate Toon Shader, Stylized Grass, Water, Fluid Glass UI, Card FX, Hologram FX. CC0.

### Effects Collection Vol.2 <span class="pill done">Owned</span>
Newer collection, **still in active development** (not all effects included yet, aims for ~Vol.1 size). Kits: Hit FX, Stylized Flame FX, Frosted Glass, Elemental Magic FX, Status FX, Electric FX, Dark Magic FX, Battle FX, Explosion FX. CC0.

---

!!! tip "Next pulls per the checklist"
    The highest-value un-wired gaps: **Loot VFX** (artifact drops), **Explosion FX** (explosive rock/meteors), **Card FX** (3-card draft), **Status FX** poison/ice/flame variants (live status auras), **Ice/Poison/Electric** (element identities), **Modular Transitions** (menu wipe).
