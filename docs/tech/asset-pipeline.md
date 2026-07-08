# Asset Pipeline

How art and models get from Blender into TowerDrop.

## Hybrid workflow

The governing rule (see [Design Pillars](../pillars.md)): **code for systems, scenes/resources for tunable visual assets.** Values meant to be *felt* — particle amounts, emission, colors, curves — live on `.tres`/scenes so they can be tuned in the Godot editor without a code round-trip.

!!! warning "Editor-conflict rule"
    Don't write a `.tscn`/`.tres` from tooling while it's open in the Godot editor — the editor clobbers it on save. Hand over the resource or the steps instead.

## Imported assets

Curated external + modeler assets live under `res://Imported/`:

- **Roll-Call pack** (`res://Imported/RollCall/`) — JMO cartoon-FX particle textures, PureNature rocks/boulders, replacement grass, two cartoon equirect skyboxes. Grass shadow fix: `cast_shadow` Off. (Licensing caveat noted with the import.)
- **New modeler tower** (`res://Imported/NewTower/`) — see below.

## Blender → Godot: the extraction pattern

The modeler works in one big `.blend` with everything named. A re-runnable Python export script pulls each named object into its own GLB, sorted into subfolders, and **never saves the `.blend`**. This is how the new tower was extracted.

- The export script sits next to the `.blend` and can be re-run when the modeler updates it.
- Blender for these runs is the **Steam install** (Blender 5.1), not on PATH.
- Godot imports the GLBs headlessly via the 4.7 console exe (`--headless --import`).

### The new tower + brick shell

`newtower.blend` was extracted into ~56 GLBs under `res://Imported/NewTower/{Tower,Cart,Props,Environment}/`. Highlights:

- Tower parts keep offsets relative to the tower axis, so they reassemble at one origin; `tower_full.glb` is the whole thing.
- Many props map directly to game items (bread, poison bread, molotov, oil, pipe bomb, torch, coins).
- The decorative **brick shell** is rebuilt as a MultiMesh from the modeler's geometry-node layout (islands split, per-island transform solved) — see [The Tower](../game/tower.md) for the runtime damage-state API.
- Watch-outs: the WIP leaf-trees instance ~18M polys of leaves (exported trunk-only); particle nails were baked before export.

## Headless checks

Use the **Godot 4.7 console exe** for headless imports and script smoke-tests. Note the headless renderer is a dummy: MultiMesh writes are no-ops and reads return identity, so **verify visuals in the editor, not headless**.

## Related

- [Art Direction](art-direction.md) — the look these assets serve.
- [The Tower & Base Defenses](../game/tower.md) — the brick-shell runtime system.
- [Engine & Tooling](engine.md) — the Godot binary and dev tools.
