# Artifacts & Relics

**Artifacts are PHYSICAL objects** (since 2026-07-19): a relic does **nothing while it sits in the bag** — it only works while **mounted on the tower**. This is the foundation of the planned **traps / hang-from-the-tower family**: special artifacts hung on the wall that run passive effects.

## The loop

1. **Get artifacts**: the main faucet is **the DRAFT (2026-07-21)** — after **every wave**, three artifact cards slam into the centre of the screen (the reusable `CardPick` chooser, time crushed to 5% while you decide) and the player **picks 1**, which lands in the bag. Wave 1's offer always includes the Coin Maker (the economy starter); duplicates across waves are welcome, since copies stack additively and the game leans hoard-of-many over few-but-powerful. Enemies also keep a small drop chance on kill (4% placeholder), and the F4 debug panel can grant randoms for testing. (The old fixed wave-1 Coin Maker grant is gone.)
2. **The bag** (right-edge ✦ tab, below the Spellbook): a **3×3 slot grid**. Owned-and-unplaced artifacts show as colour-tinted icons with a ×N badge for stacks; hover an icon for a tooltip (name, effect, bag/mounted counts); **click an icon to start placing it**. A mounted copy *leaves* the bag — the grid shows only what you can still place.
3. **Placement**: a ghost preview snaps to valid surfaces; left-click mounts, right-click/Esc cancels, and with more copies in the bag placement **continues automatically** until you run dry or cancel. A **double-click banks the held copy** back to the bag.
4. **Surfaces** — two flags per artifact: **`hang`** (the side wall: any upright surface in the wall ring, bricks included) and **`top`** (upward faces on the tower's upper half: the floor, the rim the wizards stand on, the side bits).
5. **The Mover tool** (button beside the Repair tray, debug looks for now): click a mounted artifact to pick it back up and re-place it; a quick second click sends it to the bag. On-place bonuses revert correctly on pickup.
6. **Wall risk**: an artifact hung on the wall links to the brick it hangs on (the same coverage rule the armor uses). If that brick **breaks**, the artifact is knocked loose and returns to the bag. Top mounts are safe — so wall placement is close to the action but destructible, the top is passive but protected.

## Effects: only while mounted, only while a wave is live

Tick effects (the Coin Maker) run **only during telegraph/assault** — rest, grace and menus pause the timer (no farming the downtime). Stat bonuses aggregate via `ArtifactInventory.bonus(type)` counting **mounted copies only**, and stack **additively from base** (two +5% = +10%, not compounding — decided 2026-07-19).

**The full list of artifacts and their effects lives on the [Artifact Catalogue](artifact-catalogue.md) page.** In short: six passive stat bonuses (Coin Maker, Lucky Coin, Swift Wheels, Sharp Edge, Quick Hands, Thick Bark), a set of element turrets/emitters (Frost Beam, Fire Beam, Lure Beacon, Poison Vent, Shock Coil, Dart Spitter, Oil Dripper), and one defensive relic (Crest Shield, +10 armour HP to the brick it hangs on).

The passives use the placeholder **0.5³ cube** tinted the artifact's colour; the turrets have their **own scenes** — the first users of the per-artifact `scene` key. Each catalogue entry can name its **own scene** (`scene` key; root script extends `TowerArtifact`, a `mount_offset` export seats the mesh on the surface) — that's the path to real relic models.

## Tech notes

- `Items/Scenes/tower_artifact.tscn` + `TowerArtifact` (`Items/Scripts/tower_artifact.gd`); pick colliders live on collision layer 8 **ArtifactPick**.
- `WaveManager.wave_completed` (emitted on entering REST) drives the after-wave draft (`ArtifactInventory._on_wave_completed` builds the 3-card offer and calls `CardPick.present`); `CoinSpawner.spawn_bonus_coin(pos)` is the single-coin payout.
- Catalogue + placement + bag state all live in the `ArtifactInventory` autoload; everything resets on game over (meta-progression persistence = an open call).

!!! warning "Still open"
    Trap-type effects (the actual reason the family exists), real relic models, a pretty mover cursor, placement slots/limits, and whether relics persist across runs. See the [Backlog](../project/backlog.md).

## Related

- [Enemies](../game/enemies.md): the carry system that can feed drops.
- [The Tower](../game/tower.md): what everything mounts on.
- [Backlog](../project/backlog.md): remaining artifact work.
