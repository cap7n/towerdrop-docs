# Artifact Catalogue

Every artifact currently in the game and what it does. Artifacts are **physical relics**: they do nothing in the bag and only work while **mounted on the tower**, and only while a wave is live. For how you get them (the after-every-wave draft), how the bag/placement works, and the wall-vs-top risk, see [Artifacts & Relics](artifacts.md).

Values below are the current in-game numbers (all tunable placeholders). **Mount** = where it can go: **Both** (wall or top) or **Wall only**.

Source of truth: the `ARTIFACTS` catalogue in `Autoloads/artifact_inventory.gd`.

---

## Passive bonuses

Simple stat boosts (the placeholder colour-tinted cube). Bonuses count **mounted copies only** and stack **additively** — two +5% = +10%.

| Artifact | Effect while mounted | Mount |
|---|---|---|
| **Coin Maker** | Drops a coin (arcs to the pile) every **2.5 s** during a wave. The economy starter — wave 1's draft always offers it. | Both |
| **Lucky Coin** | **+1 gold** per kill | Both |
| **Swift Wheels** | **+5%** cart speed | Both |
| **Sharp Edge** | **+1** rock damage | Both |
| **Quick Hands** | **−5%** conjure time | Both |
| **Thick Bark** | **+50** tower max HP (applies on mount, reverts on pickup) | Both |

## Turrets & emitters

Each has its own scene and reacts to nearby enemies with an element effect. Beams hold the nearest target; emitters fire on proximity.

| Artifact | Effect while mounted | Mount |
|---|---|---|
| **Frost Beam** | Holds the nearest enemy in a beam and **slows** it | Both |
| **Fire Beam** | Holds the nearest enemy in a beam and sets it **on fire** | Both |
| **Lure Beacon** | Drops a **Lure** when an enemy comes within **6 m** (~every 5 s) | Both |
| **Poison Vent** | Puffs a **poison cloud** when an enemy comes within **6 m** (~every 4 s) | Both |
| **Shock Coil** | Every **20 s**, shocks **all** enemies within **6 m** (damage + stun) | Both |
| **Dart Spitter** | Spits a **poison dart** at a nearby enemy every few seconds | Both |
| **Oil Dripper** | Dribbles a streak of **oil** down the wall below it (localized pour, not the whole-tower Oil ultimate) | Both |

## Defensive

| Artifact | Effect while mounted | Mount |
|---|---|---|
| **Crest Shield** | Reinforces the brick it hangs on with **+10 armour HP**. Hangs on the wall only — it protects the spot it sits on. | Wall only |

---

## Notes

- **Duplicates stack.** The draft welcomes repeats (hoard-of-many over few-but-powerful), so a second Coin Maker means two coin drips, a second Swift Wheels means +10% cart speed, and so on.
- **Wall vs top.** Wall (`hang`) mounts sit close to the action but ride the brick armour — if that brick breaks, the artifact is knocked loose back to the bag. Top mounts are passive but safe. Crest Shield is wall-only by design (it *is* the wall reinforcement).
- **Placeholders.** The passive bonuses use a colour-tinted cube; the turrets have their own scenes. Real relic models and the trap-family effects are still on the [Backlog](../project/backlog.md).
