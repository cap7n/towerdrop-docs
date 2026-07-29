# Performance

## The target

**200–250 enemies on screen, smooth on most machines.** That's the ceiling, not a soft goal on the way to thousands. Thousands was never a real target.

Choosing a modest cap is a **feature, not a compromise**: it keeps the door open for the expensive, feel-rich effects TowerDrop leans on (per-enemy fire, ragdoll deaths, shell pop-offs, physical debris) which a massive-horde architecture (GPU-animated instances, body-less enemies) would force us to cut.

## ⚠️ The ACTUAL bottleneck is a FIXED cost, not the horde (measured 2026-07-25)

Three machines, three captured runs, and every one of them **flat**: a tester at a steady **11-12 fps**, a second player at a steady **22**, Cap7n at a steady **47**. The tester's log is the clearest — 12 fps from second 2, in the grace phase, with **zero enemies on screen**, then 11-12 fps for the whole 522-second run. Median frame **92 ms**, p90 **97 ms**; only 3 of 522 samples exceeded 200 ms. Bucketed by live enemies: 0 alive → 12 fps, 30 alive → 12 fps. **No correlation with scene content whatsoever.**

A machine that's genuinely overwhelmed produces *variable* frame times that worsen under load. Flat-but-different-per-machine means the framerate is set by a **constant per-frame cost** that each GPU chews through at its own speed. Her GPU is an **RTX 3060** — this is not a hardware problem.

The constant costs, all per-pixel or per-texel and all independent of enemy count:

| setting | where | note |
|---|---|---|
| `directional_shadow/size = 8192` | `project.godot` | **4× the texels of Godot's 4096 default**, re-rendered every frame. First thing to try. |
| `ssao_enabled = true` | `TD_Level_Environment.tres` | Full-screen screen-space pass. |
| `glow_enabled` + 4 levels | `TD_Level_Environment.tres` | Full-screen blur chain. |
| viewport 2560×1440, fullscreen | `project.godot` | Everything above scales with this. |

*(Fog is `fog_mode = 1` (depth), not volumetric — that one is cheap, leave it.)*

**This reframes Phase 0.** The quick wins below target horde scaling; the tester's log says the horde cost her nothing. Optimising it would not have moved her framerate by one frame. **Do the fixed-cost pass FIRST**, and expose it as a quality preset in the existing settings menu so weak machines can drop it without an art argument.

Diagnostic for next time: if lowering the resolution raises the framerate, it's the per-pixel effects above. If it doesn't move at all, suspect **power throttling** (laptop on battery, or Windows graphics preference set to power-saving for the exe), which produces the same flat signature.

**BalanceLogger should record GPU / resolution / window mode** so future tester logs answer this on their own. <span class="pill todo">Todo</span>

### RESOLVED (same day): it was the foliage — 99.6% of all geometry

The F4 panel's **Foliage on/off** toggle partitioned it in one click: primitives 229M → **984k** with the scatter hidden, fps 50 → ~130. Not shadows, not SSAO, not glow — the baked ProtonScatter. Why "baked" didn't mean cheap: baking removes the CPU scatter cost, but a MultiMesh still draws **every instance at full poly, every frame** — one AABB for the whole scatter (no per-instance frustum culling) and no per-instance LOD. ~20k instances × a 12,228-tri leafy PATCH mesh = the entire frame.

**Fix shipped (commit `a9d7396`): the patch mesh decimated in place, 12,228 → 1,528 tris**, via Godot's own meshoptimizer (`tools/decimate_grass.gd`, re-runnable) — mutating the same `.res` so the existing bakes picked it up with zero level edits. Because the patch is a multi-clump bundle, decimation reads as *uniform thinning* (fewer blades per patch, full coverage kept) — better-looking than cutting instance counts, which leaves bald spots.

**Measured result: 229M → 78.7M primitives, 50 → 100 fps** on the 4090. Extrapolated to the 12-fps 3060 tester: ~25-30 fps expected. Remaining 78.7M is mostly the **trees** (which also render into 4 shadow splits) — same decimate-in-place trick applies if a round 2 is ever needed; chunking the scatter for frustum culling is the lever after that.

⚠️ ProtonScatter gotcha rediscovered: `force_rebuild_on_load = false` means the game renders only the bake **saved in the scene** — a re-shaped/chunked scatter shows in the editor (live rebuild) but ships nothing unless the rebuilt output was saved.

**Cross-machine validation (2026-07-25, same evening):** a tester who ran a steady **20 fps** on the old build reports a **stable 80** on the fixed one — ×4, right on the extrapolation. The foliage diagnosis holds across hardware.

Follow-up findings: chunk size is a real dial — 15 m cells exploded draw calls to ~13k and *lost* fps (CPU submission cost); coarse cells (~40-50 x/z, y=100 = one vertical layer) landed at ~3.4k draws / ~20M primitives / 105 fps. Draw calls are a CPU cost, primitives a GPU cost — testers' machines are GPU-bound, so primitives are the number that matters for them. **Billboard grass is ruled out** (2026-07-25, tested): the orbiting camera makes camera-facing quads visibly swivel. The Binbun quad-grass spike kit (2 tris/tuft, trample already ported — `assets/BinbunGrass/TD_*`) stays shelved for a **crossed static quad** variant someday.

### PARKED: Quality preset in the settings menu (design ready, build later)

One **Quality: Low / Medium / High dropdown** — deliberately not individual toggles (testers don't tune ten knobs, and every switch multiplies the support matrix). Levers, ranked by measured impact:

1. **Render scale + FSR** (the untouched big lever): render 3D at 0.66 / 0.85 / 1.0 and upscale — halves the GPU frame at Low on a 1440p screen; Godot's FSR is built in (`scaling_3d_mode` / `scaling_3d_scale` on the root viewport).
2. **Shadow tier**: map 1024 / 2048 / 4096 + soft-filter quality, runtime-changeable via `RenderingServer` (no restart).
3. **SSAO + glow off on Low** (the two full-screen passes in `TD_Level_Environment.tres`).
4. *(Maybe-later)* foliage density (hide alternate chunks) — only lever that changes gameplay readability, ship without it first.

Rules: **default = High** (Low is an opt-in rescue, not the first impression), and **the preset + GPU name + resolution get written into the balance logs** — that's what closes the "tester logs answer the hardware question by themselves" todo above: "feels slow" + `RTX 3060, 1440p, High` in the log = "try Medium", no profiling session. All the plumbing (settings menu, `user://settings.cfg`, apply-at-boot) already exists; the only new wiring is the FSR call.

## The real driver

A tester saw **lag around ~200 enemies**. So the job isn't to scale to a bigger number; it's to be **silky at the cap** on typical hardware.

## What that means for the roadmap

The structural rewrites from the old "scale to thousands" audit (VAT/MultiMesh GPU animation, a body-less horde) are **off the table** at this cap. What's left is the cheap **Phase 0** quick wins, worth doing before a bigger playtest but not urgent:

- Cap and pool **ragdoll / shell-debris RigidBodies** (uncapped death-churn is a surprise spike source).
- Tune **enemy shadow casting** (biggest raw draw-call sink).
- Kill the **per-tick allocation flood** in the enemy manager (the grid-rebuild + `get(cell, [])` pattern discards ~tens-of-thousands of throwaway Arrays per physics tick, invisible but murders the GC).

None of this requires surgery. It's an afternoon before a playtest, not a project.

## Why we're comfortable

The audit found the game was already built the cheap way where it counts: enemies **don't collide with each other** (separation via spatial hash), climb/attack are **manual-move** (no physics), mesh LODs are on, the enemy list is cached once per frame, and coins settle into a MultiMesh pile. The things that scale badly are per-enemy-per-frame costs, so **new features are safe as long as they don't add per-enemy per-frame work.** Per-brick HP, damage tints, popped-brick debris at 250 enemies is rounding error.

## Reference

A styled HTML perf report (`PERF_AUDIT_5000.html`) lives in the artifacts folder. Note its "5000" framing predates the confirmed 200–250 cap: read the walls-and-fixes, ignore the target number.

## Related

- [Design Pillars](../pillars.md): "right-size the ambition."
- [Enemies](../game/enemies.md): the shared enemy architecture.
