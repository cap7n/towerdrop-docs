# The Cart

The cart is the player's body in TowerDrop. It rides a **circular rail around the tower** and is the delivery system for every offensive item.

## Movement: the rail

- The cart's position is **theta-based** (cos/sin around the tower center), not a `PathFollow3D`.
- The rail's radius and height are **derived from where the cart is placed** in the level: drag the cart in the editor and the rail updates. No manual radius tuning.

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
