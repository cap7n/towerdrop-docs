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
