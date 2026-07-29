# The Cart

The cart is the player's body in TowerDrop. It rides a **circular rail around the tower** and is the delivery system for every offensive item.

## Movement: the rail

- The cart's position is **theta-based** (cos/sin around the tower center), not a `PathFollow3D`.
- The rail's radius and height are **derived from where the cart is placed** in the level: drag the cart in the editor and the rail updates. No manual radius tuning.

### Speed curve (retuned 2026-07-25)

| | m/s | why |
|---|---|---|
| base (`cart.move_speed`) | **3** | Deliberately sluggish. At the old 6 the cart could already be anywhere in time, so travel time was never a constraint and buying speed bought nothing. |
| fully upgraded | **6** | The "very good feel" target. The 6 `cart_speed_10` shop levels **double** the base — `ShopGraph.SPEED_PCT_PER_LEVEL` is `1.0/6` for exactly this, so keep it in step with the node's `max`. 2440 gold to max. |
| hard cap (`cart.max_move_speed`) | **10** | Swift Wheels artifacts stack multiplicatively ON TOP of the shop levels and the draft hands out duplicates freely — 9 copies on a maxed cart would hit 8.7 unclamped. Past ~10 it stops being controllable. |

Per level: 3.0 → 3.5 → 4.0 → 4.5 → 5.0 → 5.5 → 6.0.

!!! warning "Percentage upgrades cut both ways"
    Both speed sources are percentages of the base, so lowering the base also shrinks what each level is worth in absolute terms. Halving 6 → 3 without raising the per-level rate would have made each purchase *weaker* (+0.3 m/s instead of +0.6), which is the opposite of the goal — hence the 10% → 1/6 change in the same pass.

## Core verbs: drop, toss, pour, throw

The cart carries an **inventory** (a left-column loadout, drag-to-reorder), base 3 slots, upgradeable to 9 via the [Tab shop](tower.md). What you do with the selected item:

- **Tap = toss one**: release a single item.
- **Hold = pour all**: dump the whole stack of that item.
- **The cannon shot** (reworked 2026-07-21, was the "aimed throw"): a hold-release skill shot alongside the drop:
    - **Right-click HOLD = arm**: the selected item is magically pulled up out of the cart (higher and forward of the rim) and hovers in its arcane bubble, leaning toward wherever you aim. Arming takes `throw_lift_time` seconds — the "reload", with an `arm_time_mult` hook already wired for a future reload-speed upgrade.
    - **RELEASE = fire**: the item is SHOT ballistically at the aim point — a flat cannon trajectory (`throw_shot_speed`, 30 m/s), not a lob; from the tower top most shots angle down at the field with a small residual arc. Lands exactly on the mark and triggers on enemies mid-flight. A quick click still auto-fires the moment arming completes.
    - **The magical trajectory line**: while arming, a ribbon traces the *actual* flight path — arcane purple when the flight is clear, cut short and red where it would strike the tower. It is **advisory only**: nothing hard-blocks the shot (item hitboxes vary, so it's the player's read and their risk — a bad shot bonks the wall and drops). Releasing over the tower or sky tucks the item back into the bin (the cancel). A proper VFX pass for the line is in the [Backlog](../project/backlog.md) ("VFX for aim line").
    - **No fixed arc anymore**: the old ~60° cone gate is deleted; you can fire anywhere the flight physically clears the tower.
    - **Left-click** is still an instant throw at a hovered enemy (same cannon solve).
    - Planned next: **per-element impact identities** for aimed shots (drop = damage, shot = impact; rock pushback first).

!!! note
    The debug aim visuals are gone (2026-07-19): the magenta throw-target beam, the vertical drop line under the cart, and later (2026-07-21) the green/red ground reticle — replaced by the trajectory line above. The drop-line's anchor node still exists invisibly (items pour down its column). The cart also got its v2 model (seat, handle, chain), wheel-centred to the same rail fit.

## Feel details

- **Tilt on drop**: the cart visual is a child of a `TiltPoint` marker and pitches forward while dropping, returning on release. The held-item stack is parented under the same `TiltPoint` so it pitches with the cart.
- **Wheel sparks**: `cart_sparks` emitters fire when the cart's speed changes sharply. (Known rough edge: both rear emitters spark regardless of which wheel actually touches the rail; a restructure is parked in the [Backlog](../project/backlog.md).)

## Item behaviors on the ground

Dropped items aren't all instant-hit. The design space includes **trigger, vanish, linger, and trap** behaviors, plus collision and a tower-bounce. The [Items & Elements](items.md) page covers what each element does; the ground-behavior nuances are wired per item.

## Test scenes

- `cart_test.tscn` (F6): isolated cart/drop iteration.
- `vfx_bench.tscn` (F6): THE test bench (replaced item_bench 2026-07-21). Two tabs: VFX triggers (statuses/auras/impacts on dummy spiders + ground effects) and every item, dropped through the real effect path.

## Related

- [Items & Elements](items.md): what you're delivering.
- [The Tower & Base Defenses](tower.md): the thing you're protecting and its own upgrades.
