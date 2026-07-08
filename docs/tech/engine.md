# Engine & Tooling

## Engine

TowerDrop runs on **Godot 4.7-stable**. The engine `.exe` lives on the Desktop (not on PATH); the **4.7 console exe** is the one to use for headless imports and script checks.

The 4.7 upgrade was completed and merged to `main` with visuals confirmed; only ProtonScatter needed a one-line fix.

## Input & cheats

The input map covers cart movement, item drop/toss/pour, aimed throw, and the ultimate hotkeys (`O` oil, `I` frost, `P` slime).

- **Left-click** is consumed by different systems depending on context (build placement / wizard-open picker / TAB-shop drag), each behind its own gate — mind which owns the click in a given mode.
- **`F4` opens the DEBUG SANDBOX panel** (`DebugPanel`) — a button panel for deliberate experimentation, replacing the old "F4 = instant gold" so testers can't fat-finger free money. `DEBUG_TOOLS` is deliberately kept **on** for the tester build (the sandbox is a feature). Some raw keys still live (F1 gold drip, F5 spend, F6 coin burst, Enter skip wave, fluid-tuning keys) pending a fold-into-panel cleanup.
- The **balance overlay** is on the backtick key.

## Balance logger

A **`BalanceLogger` autoload** writes economy/pressure CSVs every run — gold, tower HP, incoming enemy HP vs. throughput — for the feel-first-then-numbers tuning approach. Logs collect under the artifacts folder's `balance_logs/`. See [Economy & Gold](../game/economy.md).

## Design tools (HTML)

Standalone browser tools (kept in the artifacts folder, not the game project):

- `Tools/skill_tree_maker.html` — authors the six [element trees](../game/items.md); syncs with the in-game shop.
- `PERF_AUDIT_5000.html` — the perf report (see [Performance](performance.md); ignore its outdated "5000" target).
- `recipe_map.html`, `shop_progression.html`, `unlock_tree.html` — older progression visualizations.

## Workspace layout

- **Game project:** `Desktop\tower-cart-game` — keep this clean (code + game assets only).
- **Artifacts / AI workspace:** `Desktop\Tower-Cart-Game-Claude` — HTML tools, docs, mindmaps, balance logs, Blender source models.

!!! note "This wiki is the new home for planning"
    Loose planning docs (backlog, progress, build plans) are being consolidated **into this wiki** as the single point of truth, mirroring the approach used for the OTR project.

## Related

- [Asset Pipeline](asset-pipeline.md) — headless Godot + Blender extraction.
- [Performance](performance.md) — the perf target and quick wins.
