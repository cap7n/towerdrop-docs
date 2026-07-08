# Design Pillars

The taste and constraints TowerDrop is designed through. These are strong defaults, not laws, but break them on purpose, not by accident.

## 1. Base-game fun before progression polish

The priority is the **moment-to-moment fun and feel** of defending the tower from the cart: the aiming, the impacts, the "oh no the wave is on the far side" scramble. The messy parts of the upgrade/progression systems are **parked, not rejected**: don't spend the session cleaning up trees and economy while the core loop still has feel wins on the table.

## 2. Physical feedback over abstract feedback

Prefer **physical, physics-driven feedback** to abstract bursts. A spider that ragdolls and tumbles off the wall reads better than a poof of particles at the death location. Weight, knockback, things sliding and falling: that's the language. This is why the on-hit health bars and floating status text were removed in favor of body-tint hit-flash and ragdoll deaths.

## 3. Stylized, and **no outlines**

The look is **stylized, leaning faceted-flat / soft-matte**. Explicitly **no cartoon outlines**: they've been tried and disliked. The current nature/prop assets are placeholders; the real art pass waits for final models. Parked toon-shader experiments live in `VFX/Shaders/toon*.gdshader` but are not the committed direction. See **[Art Direction](tech/art-direction.md)**.

## 4. Hybrid workflow: code for systems, scenes for tunable visuals

**Systems are built in code; tunable visual assets are built as scenes/resources** so they can be adjusted in the Godot editor without a code round-trip. As a rule of thumb: if a value wants to be felt and eyeballed (particle amounts, emission, colors, curves), it belongs on a `.tres`/scene the editor can tweak. If it's logic, it's code. See **[Asset Pipeline](tech/asset-pipeline.md)**.

!!! warning "Editor-conflict rule"
    Don't edit a `.tscn`/`.tres` from tooling while it's open in the Godot editor: the editor will clobber it on save. When a change is to an open scene, hand over the edited resource / the exact steps instead of writing the file underneath the editor.

## 5. Right-size the ambition

The enemy count target is **200–250 on screen, smooth on most machines**, not thousands. This keeps the door open for the expensive, feel-rich effects (per-enemy fire, ragdolls, shell pop-offs) that a massive-horde architecture would force us to cut. See **[Performance](tech/performance.md)**.

## Working-relationship notes

- **Steer by feel, then numbers.** For balance, get the feel right and let the [balance logger](tech/engine.md) CSVs inform the numbers, not the other way around.
- **Don't allow lazy shortcuts** when the hybrid workflow (proper scene/resource authoring) is the right call.
