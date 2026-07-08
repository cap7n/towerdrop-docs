# Technical Solutions

A log of specific implementation problems and how they were solved. This is the "how" that game-design pages deliberately leave out; if you're reading a page like [Enemies](../game/enemies.md) or [Economy & Gold](../game/economy.md) and want the engineering story behind a feature, it lives here.

## Reparenting broke node references (Tower)

**Problem:** fixed `$NodePath` lookups between scripts break the moment a scene gets moved or reparented, which made restructuring the tower risky.

**Fix:** the tower is packed into its own `GameTower` scene, and cross-script references are **group-based** rather than path-based, so the tower can be reparented without breaking wiring. A `spikes_anchor` Marker3D positions the spike ring so it isn't hardcoded to a specific node path either.

## Coin pile at scale

**Problem:** kills can drop thousands of coins over a run. Keeping every coin as a live `RigidBody3D` forever would wreck performance; converting them to something static immediately would lose the satisfying arc-and-settle physics.

**Fix:** a coin is a real `RigidBody3D` **only while it's falling**. The moment it settles, it's absorbed into a MultiMesh + heightmap coin pile and the physics node is freed. Fast-falling coins use continuous collision (`continuous_cd`) so they don't tunnel through the thin heightmap on the way down. See [Economy & Gold](../game/economy.md).

## Spider fire trailing behind the body

**Problem:** an early version of the spider's body-fire used world-space particle emission, which left the flame hovering near where the enemy used to be instead of riding along with the moving mesh.

**Fix:** switch the process material to **local coordinates** (`local_coords = true`) so the flame follows the body transform. The material is baked externally and shared across spiders so the bake sticks. See [Atmosphere & VFX](../systems/atmosphere.md).

## Spider gait (historical, superseded)

**Problem:** the spider needed a climbing gait that plants its feet realistically against a curved wall, but Blender's IK solver doesn't run headless, which blocks any pipeline that wants to bake animation via script.

**Fix at the time:** the gait was hand-solved with an **analytic 2-bone IK implementation in Python**, baked by iterating through an editable source file (`spider_walk_editable.blend`) with a bake script, rather than relying on Blender's built-in IK.

**Current status:** this approach has been **replaced**. The live spider now runs a **baked walk/climb animation**, not a live analytic IK gait. The lesson (Blender IK + headless don't mix) is kept here in case a future enemy's locomotion runs into the same wall.

## Brick-shell extraction from a realized mesh

See [Asset Pipeline](asset-pipeline.md) for how the new tower's decorative brick shell was reconstructed as a MultiMesh from the modeler's baked geometry-node layout.
