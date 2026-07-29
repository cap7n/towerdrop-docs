# Meta-Progression: the stat menu

A gathered list of **every variable in the game that would make a good skill-tree node**, pulled from the current code (not from the design intent). This is the raw material for populating the persistent meta tree — pick from here, don't invent numbers that have no home in the codebase.

For *why* a meta layer exists and how it persists, see the [Backlog → Meta-progression](../project/backlog.md) section. For the per-run trees this must not duplicate, see [Items & Elements](../game/items.md) and [The Tower](../game/tower.md).

---

## How to read the tables

Every row names the **actual variable** and the file it lives in, so a node can be wired without a hunt.

| Column | Meaning |
|---|---|
| **Now** | The value in the code today (the base a meta bonus multiplies or adds to). |
| **Hook** | 🟢 = a single place already reads it, wiring is a one-liner. 🟡 = read in several places, needs a small helper. 🔴 = no hook exists yet, the meta node has to build one. |

!!! warning "The double-dip rule"
    Several of these are **already upgradeable inside a run** (TAB shop, wizard trees, artifacts). A meta node on the same stat is fine — it's the classic "your run starts stronger" shape — but decide per stat whether meta is **additive to the base** (before in-run scaling) or a **final multiplier** (after). The existing code has a precedent worth copying: `ShopGraph` captures `_base_speed` / `_base_inv` at `_ready()` and always scales *from the base*, so a meta bonus applied to `cart.move_speed` before the shop reads it flows through cleanly. Anything applied *after* fights the shop for the same field.

---

## A. The cart — the player's body

The most directly *felt* upgrades. `Characters/Player/Scripts/cart.gd`.

| Stat | Variable | Now | Hook | Notes |
|---|---|---|---|---|
| **Move speed** | `move_speed` | 3.0 | 🟢 | The headline cart stat. Deliberately sluggish so speed is worth buying. TAB shop takes it to 6.0 over 6 levels; Swift Wheels artifacts stack on top. |
| **Speed ceiling** | `max_move_speed` | 10.0 | 🟢 | The hard cap ("past this it stops being controllable"). A meta node that *raises the cap* is a different, spicier reward than one that raises speed. |
| **Acceleration** | `acceleration` / `deceleration` | 14 / 18 | 🟢 | Responsiveness rather than top speed — a "handling" node. Reads as snappier without breaking the speed cap. |
| **Reverse braking** | `reverse_deceleration_multiplier` | 2.5 | 🟢 | How fast you can reverse direction on the rail. Directly targets the core failure state (being on the wrong arc). |
| **Inventory slots** | `inventory_capacity` | 3 | 🟢 | Starting loadout size. TAB shop buys +1/level. |
| **Inventory ceiling** | `max_inventory` | 9 | 🟢 | Hard cap on slots (9 = one 3×3 bin layer — raising past 9 needs a bin re-layout). |
| **Cannon reload** | `arm_time_mult` × `throw_lift_time` | 1.0 × 0.25 s | 🟢 | **`arm_time_mult` already exists and nothing writes to it** — it was put in explicitly as a reload-speed upgrade hook. Cheapest real node in the game. |
| **Shot speed / flatness** | `throw_shot_speed` | 30 m/s | 🟢 | Higher = flatter, faster, punchier cannon. Also shortens time-to-impact on far targets. |
| **Pour rate** | `pour_interval` | 0.12 s | 🟢 | Seconds between items while dumping a stack. A "dump the whole bin faster" node. |
| **Tap window** | `tap_max_time` | 0.18 s | 🟡 | Feel knob, not power. Probably belongs in settings, not a tree. |
| **Toss speed** | `toss_duration` | 0.32 s | 🟢 | How long a single tossed item takes to arc out of the bin. |
| **Tilt speed** | `tilt_speed` / `dump_tilt_threshold` | 8.0 / 0.9 | 🟢 | How quickly the cart commits to a pour. Pairs with `pour_interval` as a "Fast Hands" node. |
| **Trap launch** | `trap_throw_speed` | 3.5 | 🟢 | How far out trap items land. Only matters once the trap family exists. |

## B. The tower & its armour

`Levels/Scripts/tower_core.gd`, `Components/health.gd`, `Imported/NewTower/Tower/tower_brick_shell.gd`.

| Stat | Variable | Now | Hook | Notes |
|---|---|---|---|---|
| **Core max HP** | `Health.max_hp` on the tower | 500 (`game_tower.tscn`) | 🟢 | The run-length dial. TAB shop adds +50/level ×10; Thick Bark adds +50 per mounted copy. |
| **Brick armour HP** | `brick_max_hp` | 10.0 | 🟢 | Per-brick armour, ×110 bricks. The *real* effective HP pool — attacks are soaked here before the core. Very strong lever, tune gently. |
| **Armour coverage** | `armor_coverage_radius` | 4.5 | 🟡 | How wide a hit spreads across neighbouring bricks. Bigger radius = damage smeared over more bricks = softer local breaches. |
| **Repair cost** | `RepairController.GOLD_PER_HP` | 1.0 g/HP | 🟢 | A discount node is a clean economy-flavoured meta reward. |
| **Regen amount / interval** | `TowerCore.regen_amount` / `regen_interval` | spell-driven (0 when idle) | 🔴 | ⚠️ **Do not add a passive meta regen.** Always-on regen + spikes is exactly what broke the C4 wave-39 run and got both pulled from the TAB tree. If regen is a meta node at all, make it *cheaper/longer Living Stone casts*, not a permanent trickle. |
| **Living Stone heal** | `UltimateBook.LIVING_STONE_HEAL_PER_LEVEL` / `_TICK` | 2.0 / 1.0 s | 🟢 | The safe version of the above — it only runs while the spell holds. |
| **Spike ring** | `SpikeRing.damage` / `poison_dps` / `poison_duration` / `radius` / `tick_interval` | 1.0 / 5.0 / 6 s / 9 / 1 s | 🟡 | Same warning as regen — spikes are being re-homed as a *costed Rock spell*, not a passive. Meta should buff the spell, not switch it on. |

## C. Economy & gold

`Autoloads/globals.gd`, `Characters/Enemies/Scripts/enemy.gd`, `Levels/Scripts/coin_spawner.gd`.

| Stat | Variable | Now | Hook | Notes |
|---|---|---|---|---|
| **Starting gold** | `Globals.gold` at `reset_run()` | 0 | 🟢 | The single most conventional meta node in the genre. Trivial to wire. |
| **Gold per kill** | `CoinSpawner.coins_per_kill` | 1 | 🟢 | Flat bonus on top of the HP curve. Lucky Coin artifacts already add here via `bonus(&"gold_per_kill")`. |
| **Bonus-coin chance** | `bonus_coin_chance` / `bonus_coin_count` | 0.3 / 1 | 🟢 | A "lucky strike" node — reads as juicier than a flat +1 because you *see* the extra coin. |
| **Kill payout curve** | `Enemy.coin_payout(hp)` | 1 g @1 HP → 40 g @15 HP | 🟡 | A global `×gold` multiplier applied here scales with the HP ramp automatically. The strongest economy node available; treat as a late/expensive meta tier. |
| **Shop prices** | `costs` arrays in `ShopGraph.NODES` + every wizard tree | per-node | 🔴 | A "−X% upgrade cost" node needs a single discount multiplier threaded through `_next_cost()`. Two call sites (`shop_graph.gd`, `wizard_shop_ui.gd`) — cheap, and it's a classic meta reward. |
| **Respec refund** | `GoblinShop.RESPEC_REFUND` | 0.75 | 🟢 | Raising it toward 1.0 is a "free experimentation" node. Nice non-power reward. |
| **Artifact shards** | `meta["shards"]` (`{balance, lifetime}`) | 0 | 🔴 | **Already in `meta.json` defaults with no earn source.** This is the meta *currency* the tree would spend — the open question is where shards come from, not where they live. |

## D. Wizards & item supply

`Characters/Goblins/Scripts/goblin_shop.gd`. The wizards are the item faucet — anything here directly sets how much ammo you get.

| Stat | Variable | Now | Hook | Notes |
|---|---|---|---|---|
| **Starting wizards** | `hire_wizard` levels (`ShopGraph`) | 1, +4 buyable | 🟡 | "Start with 2 wizards" is a huge power spike — it doubles item throughput. Expensive tier. |
| **Conjure time (per element)** | `base_conjure_time(e)` | rock 6 · fire 15 · poison 15 · frost 6 · electric 30 · lure 45 s | 🟢 | Per-element supply rate. Because the six values differ so wildly, a *per-element* meta discount is genuinely different from a global one. |
| **Global conjure discount** | `ArtifactInventory.bonus(&"conjure_pct")` | −5% per Quick Hands | 🟢 | The multiplier is already applied in `_effective_summon_duration()` and floored at 0.25 — a meta bonus can ride the same path. |
| **Conjure floor** | `maxf(d, 0.4)` in `_effective_summon_duration` | 0.4 s | 🟢 | Raising the ceiling on how fast supply can get. |
| **Rock base damage** | `ROCK_BASE_DAMAGE` | 2.0 | 🟢 | The one damage number every run starts with. Sharp Edge artifacts add here too. |
| **Bigger-rock scaling** | `BIGGER_DMG_FLAT` / `BIGGER_SIZE_PER_LEVEL` | 1.0 / 0.25 | 🟢 | Makes the in-run rock tree pay more per level — a "your upgrades are worth more" shape, which is a nicer meta feeling than raw stats. |
| **Scatter shards** | `SCATTER_SHARDS` | 4 | 🟢 | |
| **Delivery time** | `DELIVER_TIME` | 0.5 s | 🟢 | Time for a conjured item to travel wizard → cart. Minor. |

## E. Global effect dials

`Autoloads/globals.gd` — these two exist *specifically* as one-stop multipliers, which makes them the single best-value meta nodes in the codebase.

| Stat | Variable | Now | Hook | Notes |
|---|---|---|---|---|
| **Status duration ×** | `effect_duration_mult` | 1.6 | 🟢 | Multiplies **every** status duration at `enemy.apply_status`. One node, six elements affected. |
| **AoE radius ×** | `aoe_radius_mult` | 1.4 | 🟢 | Multiplies every pool / explosion / cloud radius at the spawn sites. Same deal. |
| **Per-item damage bonus** | `Globals.item_damage_bonus` (dict) | empty | 🟢 | Already a live hook: `DamageEffect` adds it on top of base damage. Was built for the deleted recipe system and is sitting unused — a meta node could write into it directly. |
| **Guaranteed item spawns** | `Globals.guaranteed_item_spawns` (dict) | empty | 🟢 | Same shape: guarantee N of an item per wave. Currently only the shop writes it. |
| **Starting item pool** | `_STARTER_NAMES` / `unlocked_items` | Rock in pool, 5 buyable | 🟡 | **"Start with X unlocked" is a non-numeric meta node** — arguably more interesting than +5%, because it changes the opening rather than the math. |

## F. Statuses

`Autoloads/status_db.gd`. Every status is a data row, so a meta node buffing one is a dictionary write.

| Status | Tunables | Now |
|---|---|---|
| **Slow** | `speed` | 0.5 |
| **Stunned** | `speed` | 0.0 |
| **Bleed** | `dps` | 2.0 (PURE) |
| **Poisoned** | `speed`, `dps` | 0.6, 2.0 |
| **On fire** | `dps`, `spread_radius`, `spread_duration` | 3.0, 3.0 m, 2.0 s |
| **Frozen** | `vuln` (+ `vuln_types` PURE/ELECTRIC) | ×2.0 |
| **Deep frozen** | `vuln` | ×4.0 |
| **Oiled** | `speed`, `flammable` | 0.6, true |

Hook is 🟢 across the board (one `DEFS` dictionary), but note **`vuln` on frozen/deep-frozen is the combo multiplier** — the frost→rock shatter payoff. Buffing it meta-side changes the whole combo economy, so treat it as a deliberate capstone rather than a filler node.

## G. Spells & ultimates

`Autoloads/ultimate_book.gd`.

| Stat | Variable | Now | Hook | Notes |
|---|---|---|---|---|
| **Cast cost** | `COST` | 15 gold | 🟢 | Flat placeholder across all spells. |
| **Cast duration** | `DURATION` | 10 s | 🟢 | Base before the per-element booster nodes (+2 s/level). |
| **Cooldowns** | per-spell (`quiker_pulses`, etc.) | tree-driven | 🟡 | |
| **Spell unlocks** | `Achievements.DEFS` `ultimate` key | 1 row wired (`meteors`) | 🟢 | **Already built as data.** A meta tree that unlocks spells needs no new plumbing — it appends to the same `artifacts_unlocked`-style bank. Gating stacks *on top of* the wizard-tree node, which is the shape Yaro asked for. |

## H. Artifacts

`Autoloads/artifact_inventory.gd`. The artifact layer is where meta is already half-built.

| Stat | Variable | Now | Hook | Notes |
|---|---|---|---|---|
| **Draft cards offered** | `CardPick.present` call | 3 | 🟢 | "+1 card in every draft" is a strong, very legible meta node. |
| **Draft rerolls** | — | none | 🔴 | Doesn't exist. Would be a new mechanic, but a natural meta reward. |
| **Kill drop chance** | `drop_chance_step_pct` / `drop_chance_cap_pct` | 0.1 / 4.0 % | 🟢 | Pity-counter based, with a simulated-runs table in the source comments — meaning the balance impact of a meta bump is already measurable. |
| **Per-type drop weight** | `DROP_STEP_BY_TYPE` | spider 1.0, pillbug/snail 3.0 | 🟢 | |
| **Artifact unlocks** | `Achievements.is_artifact_unlocked` | 2 gated ids | 🟢 | Route 2 (achievements) is live. Routes 1 (playtime) and 3 (shards) are the ones a tree would drive. |
| **Bag grid size** | 3×3 | 9 | 🟡 | Placement limits aren't implemented yet, so bag size is currently cosmetic. |
| **Per-artifact values** | `value` in the `ARTIFACTS` rows | +1 gold, +5% speed, +1 rock dmg, −5% conjure, +50 HP, +10 brick HP | 🟢 | A meta node that makes *artifacts themselves* stronger is a good late tier — it scales with the hoard-of-many design rather than replacing it. |

## I. The difficulty side (inverse knobs)

`Levels/Scripts/wave_manager.gd`. These aren't player buffs, but a meta tree can absolutely sell "the horde is a little kinder" — and these are the exact variables that would move.

| Stat | Variable | Now |
|---|---|---|
| Rest length | `rest_time` / `auto_next_rest_time` | 60 / 5 s |
| Telegraph warning | `telegraph_time` | 4.0 s |
| Spawn rate | `spawn_interval` | 0.45 s |
| Threat budget | `gen_budget_base` / `gen_budget_growth` | 100 / +10 per wave |
| Spider HP curve | `spider_hp_base` / `_growth` / `_cap` | 2.0 / 0.4 / 15 |
| Immunity rolls | `immune_chance_base` / `_growth` / `_max` | 8% / +1% / 45% |
| Pure-shield rolls | `shield_chance_base` / `_growth` / `_max` / `shield_hp` | 5% / +0.8% / 35% / 8 |
| Swarm waves | `swarm_every` / `swarm_start_wave` / `swarm_count` | every 5 from w15, 120 enemies |
| Enemy attack damage | `Enemy.attack_damage` | 1.0 |
| Enemy walk / climb speed | `walk_speed` / `climb_speed` | 1.5 / 0.5 |
| Enemy sprint | `sprint_mult` / `sprint_near` / `sprint_far` | ×3 / 15 m / 40 m |
| Fall damage | `fall_damage_max` | 4.0 |
| Enemy resistance | `resist_multiplier` | 0.0 |

!!! note "Careful here"
    Making the game easier via meta is a legitimate design choice but it fights the [difficulty ramp](../game/waves.md) directly and the wave generator was tuned off wave-50 stress logs. Prefer *player-side* nodes; use this table when a node explicitly wants to be "the world is gentler."

## J. Things worth stealing that aren't numbers

The most interesting meta nodes in this codebase probably aren't `+5%`:

- **Start with a wizard already specialized** into an element (`GoblinShop.element` + `specialized`) — skips the opening ramp entirely.
- **Start with N tree points** pre-spent in a wizard tree (`_tree_levels` is a plain dict, restorable — `RunCheckpoint` already does exactly this).
- **Start with an artifact mounted** (`ArtifactInventory` grant + place on run start).
- **Start with items unlocked** beyond Rock (`Globals.unlock_item`).
- **Unlock an ultimate permanently** (the `Achievements.DEFS` `ultimate` key, already data-driven).
- **Keep a fraction of gold across runs** — needs a new hook, but it's the cleanest bridge between the run economy and the shard economy.

---

## Suggested first tree (if you want a shortlist)

Everything below is 🟢 — no new plumbing, all one-liners against existing fields.

| Tier | Node | Field |
|---|---|---|
| Opening | Starting gold | `Globals.gold` at `reset_run()` |
| Opening | Cart speed base | `cart.move_speed` |
| Opening | Cart slot | `cart.inventory_capacity` |
| Opening | Tower max HP | tower `Health.max_hp` |
| Mid | Reload speed | `cart.arm_time_mult` ← *already hooked, unused* |
| Mid | Conjure discount | `conjure_pct` bonus path |
| Mid | Rock damage | `ROCK_BASE_DAMAGE` |
| Mid | Brick armour HP | `brick_max_hp` |
| Mid | Repair discount | `GOLD_PER_HP` |
| Mid | Effect duration | `Globals.effect_duration_mult` |
| Mid | Effect radius | `Globals.aoe_radius_mult` |
| Late | Draft +1 card | `CardPick.present` offer size |
| Late | Kill payout × | `Enemy.coin_payout` |
| Late | Speed cap raise | `cart.max_move_speed` |
| Late | Extra starting wizard | `hire_wizard` seed |
| Capstone | Frozen `vuln` | `StatusDB.DEFS` |
| Capstone | Start specialized | `GoblinShop.element` |

---

## Open questions this list surfaces

1. **What does the meta tree spend?** `shards` exists in `meta.json` with no earn source ([Backlog](../project/backlog.md) candidates: elite/carrier drops, wave-clear bonus, game-over conversion). Nothing else here can be designed until that's answered.
2. **Additive-to-base or final multiplier?** See the double-dip rule above. Needs one project-wide answer, not a per-node one.
3. **Does meta touch difficulty at all**, or is it strictly player-side? (Section I.)
4. **Is `arm_time_mult` the template?** It's the only field in the game already sitting there waiting for an upgrade to write to it. If meta bonuses all landed as `*_mult` fields owned by the systems themselves, wiring would stay a one-liner per node forever.

## Related

- [Backlog → Meta-progression](../project/backlog.md): the persistence layer, unlock routes, and what's already built.
- [Artifacts & Relics](artifacts.md) / [Artifact Catalogue](artifact-catalogue.md): the half-built meta layer.
- [The Tower & Base Defenses](../game/tower.md): the TAB shop this must not duplicate.
- [Items & Elements](../game/items.md): the six per-run wizard trees.
