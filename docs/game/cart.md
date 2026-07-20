# The Cart

The cart is the player's body in TowerDrop. It rides a **circular rail around the tower** and is the delivery system for every offensive item.

## Movement: the rail

- The cart's position is **theta-based** (cos/sin around the tower center), not a `PathFollow3D`.
- The rail's radius and height are **derived from where the cart is placed** in the level: drag the cart in the editor and the rail updates. No manual radius tuning.

## Core verbs: drop, toss, pour, throw

The cart carries an **inventory** (a left-column loadout, drag-to-reorder), base 3 slots, upgradeable to 9 via the [Tab shop](tower.md). What you do with the selected item:

- **Tap = toss one**: release a single item.
- **Hold = pour all**: dump the whole stack of that item.
- **Aimed throw** (prototype, in testing): instead of just dropping straight down:
    - **Right-click** lobs the selected item at a point on the terrain.
    - **Left-click** lobs it at a hovered enemy.
    - Throws are constrained to a ~60° arc, use a magical lift into a ballistic peak-arc, and land accurately (a `damp = 0` fix made the arc land where aimed). Items can trigger on enemies mid-flight.

!!! note
    The debug aim visuals are gone (2026-07-19): both the magenta throw-target beam and the vertical drop line under the cart were removed as clutter. The green/red ground reticle for aimed throws is the one remaining aim UI. The drop-line's anchor node still exists invisibly (items pour down its column). The cart also got its v2 model (seat, handle, chain), wheel-centred to the same rail fit.

## Feel details

- **Tilt on drop**: the cart visual is a child of a `TiltPoint` marker and pitches forward while dropping, returning on release. The held-item stack is parented under the same `TiltPoint` so it pitches with the cart.
- **Wheel sparks**: `cart_sparks` emitters fire when the cart's speed changes sharply. (Known rough edge: both rear emitters spark regardless of which wheel actually touches the rail; a restructure is parked in the [Backlog](../project/backlog.md).)

## Item behaviors on the ground

Dropped items aren't all instant-hit. The design space includes **trigger, vanish, linger, and trap** behaviors, plus collision and a tower-bounce. The [Items & Elements](items.md) page covers what each element does; the ground-behavior nuances are wired per item.

## Test scenes

- `cart_test.tscn` (F6): isolated cart/drop iteration.
- `item_bench.tscn` (F6): lists every item; drop on the ground or onto stationary dummy spiders through the real effect path.

## Related

- [Items & Elements](items.md): what you're delivering.
- [The Tower & Base Defenses](tower.md): the thing you're protecting and its own upgrades.
