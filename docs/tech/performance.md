# Performance

## The target

**200–250 enemies on screen, smooth on most machines.** That's the ceiling — not a soft goal on the way to thousands. Thousands was never a real target.

Choosing a modest cap is a **feature, not a compromise**: it keeps the door open for the expensive, feel-rich effects TowerDrop leans on — per-enemy fire, ragdoll deaths, shell pop-offs, physical debris — which a massive-horde architecture (GPU-animated instances, body-less enemies) would force us to cut.

## The real driver

A tester saw **lag around ~200 enemies**. So the job isn't to scale to a bigger number; it's to be **silky at the cap** on typical hardware.

## What that means for the roadmap

The structural rewrites from the old "scale to thousands" audit — VAT/MultiMesh GPU animation, a body-less horde — are **off the table** at this cap. What's left is the cheap **Phase 0** quick wins, worth doing before a bigger playtest but not urgent:

- Cap and pool **ragdoll / shell-debris RigidBodies** (uncapped death-churn is a surprise spike source).
- Tune **enemy shadow casting** (biggest raw draw-call sink).
- Kill the **per-tick allocation flood** in the enemy manager (the grid-rebuild + `get(cell, [])` pattern discards ~tens-of-thousands of throwaway Arrays per physics tick — invisible but murders the GC).

None of this requires surgery. It's an afternoon before a playtest, not a project.

## Why we're comfortable

The audit found the game was already built the cheap way where it counts: enemies **don't collide with each other** (separation via spatial hash), climb/attack are **manual-move** (no physics), mesh LODs are on, the enemy list is cached once per frame, and coins settle into a MultiMesh pile. The things that scale badly are per-enemy-per-frame costs — so **new features are safe as long as they don't add per-enemy per-frame work.** Per-brick HP, damage tints, popped-brick debris at 250 enemies is rounding error.

## Reference

A styled HTML perf report (`PERF_AUDIT_5000.html`) lives in the artifacts folder. Note its "5000" framing predates the confirmed 200–250 cap — read the walls-and-fixes, ignore the target number.

## Related

- [Design Pillars](../pillars.md) — "right-size the ambition."
- [Enemies](../game/enemies.md) — the shared enemy architecture.
