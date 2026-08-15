# Work Log

What happened, in order. Entries are appended by the task bot when a card is
marked Done - the richness of an entry tracks the richness of the Done note,
by design. Curated pages hold the WHY; this page holds the WHAT.
Bot design: [The Task Bot](../tech/wiki-bot.md).

- **2026-08-08** Captn finished **Brick repair: 1 HP per click (upgradeable rate)** _(task #1)_
- **2026-08-08** Captn finished **Keep artifacts from your last run (meta stat)** _(task #24, 0.11)_
- **2026-08-09** Captn finished **Request ~12 Steam beta keys and hand out to playtesters** _(task #82, 0.11)_
- **2026-08-09** Captn finished **Cracked brick variants for tower damage states** _(task #21)_
- **2026-08-09** Captn finished **Pillbug (roller) aiming is un-fun** _(task #45, 0.13)_
- **2026-08-13** Captn finished **Wire per-level wizard costs into the shop UI** _(task #33, 0.12)_
    - The plan said: Wire per-level wizard costs into the shop UI. wizard_shop_ui._node_cost already reads the costs[] array for the element trees; this is the remaining per-LEVEL wiring so a node's 2nd and 3rd level charge their own prices rather than repeating the first. Rebalancing the actual numbers is a separate tuning job.
    - Still open: Cost wired in, Cost now scales as displayed
- **2026-08-13** Captn finished **Ground fluid v2: eyeball and tune** _(task #68)_
- **2026-08-13** Captn finished **Dead-code Pass 3: stop the rot** _(task #73)_
- **2026-08-13** Captn finished **Purge remaining dead recipe-era Items/Data .tres (~55 files)** _(task #114)_
- **2026-08-13** Captn finished **Slim enemy.gd down: split into files, strip dead code** _(task #107, 0.13)_
    - Did: Split enemy.gd 998 to ~420 lines into three code-created Components (no scene edits): enemy_statuses.gd (status dict, DoT tick, multipliers, spread), enemy_body_fx.gd (status overlays and effects, blood drip, powerup halo, belly tint), enemy_death_effects.gd (spread-death/shatter/overload bursts). Public API kept as facades; Vfx ragdoll tint repointed. Verified by direct level_1 headless boot, zero script errors.
    - The plan said: Slim enemy.gd down: split into files, strip dead code. Follow-up to #53. After the legacy inline path is deleted, enemy.gd is still ~1300 lines of mixed concerns: status system, body/status VFX toggles, death juice (spread/shatter/overload), powerup auras, belly tint, coin payout, size roll. Break it into focused files (status handling, body VFX, death-side-effects at minimum), delete anything unreferenced once the components own their piece. The Components/ folder is the pattern to follow.
- **2026-08-14** Captn finished **FIX; raytrace fix on tower cliping** _(task #118, 0.11)_
    - Did: you can not trow threw stuff anymore, Some cliping might happen but not distubingly so
    - The plan said: FIX; raytrace fix on tower cliping. Fix the fact you can trow shit trew the tower.
- **2026-08-14** Captn finished **Remove Colour shading from damaged Schell blocks** _(task #115, 0.11)_
    - Did: colour removed and fixed Some dependency
    - The plan said: Remove Colour shading from damaged Schell blocks. They where just to test if dmg is actualy happening.
- **2026-08-14** Captn finished **BUG: artifacts could be hung on missing bricks** _(task #121, 0.11)_
    - Did: Added ArtifactInventory._mount_brick(pos), now the single source of truth for both the ghost preview and the commit - when those disagreed you got a green ghost that mounted unlinked. It asks nearest_brick() with include_gone=true so the slot that geometrically OWNS the point answers (a hole reports itself as GONE instead of deflecting to a standing neighbour) and refuses a GONE slot. _update_ghost now paints HANG spots red over a gap; TOP mounts never link and are untouched. Also fixed a world/local mismatch in the same check: it compared a global artifact position against get_brick_transform().origin, which is shell-LOCAL, so it only held while the shell sat at the world origin - now uses get_brick_world_position(). Updated the stale linked_brick doc comment that described the hole as intended. Verified by reading the call paths (restore path still links, since RunCheckpoint mounts artifacts before bricks get raw HP); in-editor check is placing over a chewed gap.
    - The plan said: BUG: artifacts could be hung on missing bricks. The placement ghost stayed green over a hole in the brick shell
