# Mana & The Grinder

<span class="pill todo">Todo</span> Designed 2026-08-28 (late-night session, Cap7n + Claude, straight off the first tester data). **Design locked, not yet built.** Board tasks exist; this page is the why and the shape.

---

## The problem this solves

Spells cost a flat 15 gold (a placeholder that shipped). The first two testers both found the same degenerate loop: **cast the repair spell nonstop, including during rest, and the tower never really loses HP.** The deeper issue is structural and no gold price fixes it: a rich late-game player casts infinitely at any price, and a poor early player never casts at all. Casting frequency was accidentally an income side effect.

At the same time, Lucky Coin (+1 gold per kill per placed coin) turned 100-kill waves into a money press (one tester wave printed ~15k gold). Balancing a per-kill gold multiplier is a forever job.

Both problems die together: **gold builds, mana casts.**

## The design

- **Mana is a single capped bar.** Kills feed it. It is spent by every spellbook cast; spells cost no gold at all.
- **The cap is the whole point.** Use-it-or-lose-it during fights encourages casting (fun goes up) while making hoard-then-trivialize impossible. The cap is sacred: upgrades may raise it, nothing removes it.
- **Kills feed it, so it self-paces.** Big late waves arrive together with the mana to fight them. Magic scales with threat, never with bank balance.
- **Rest casting is allowed** but spends a bar that will not refill until enemies show up. Heal now or hold for the fight is a real decision.
- **Lucky Coin is removed.** Its model was a grinder before it was a coin; it becomes the **Mana Grinder** artifact. Enemies go in, magic comes out. Coin Maker remains the economy artifact.
- Base mana gain exists **without** the artifact (a spell build must not be brickable by draft luck); placed Grinders boost the gain and/or the cap.

## First-pass numbers (all tunable)

| Knob | Value |
|---|---|
| Mana per kill (base) | 1 |
| Cap (base) | 60 |
| Small spell | ~15 |
| Repair spell | ~25 |
| Ultimate | ~50 (a real bite of the bar) |
| Mana Grinder (each placed) | suggestion: +0.5 gain per kill or +20 cap - pick in tuning |

A 60-kill wave fills the bar about one and a half times: several small casts, or one ultimate plus change.

## Consequences

- The flat 15g spell cost dies. `Globals.gold` and casting fully decouple.
- The cap is a new upgrade axis (tree nodes, meta levels, Grinder stacks) - always raising, never removing.
- No save migration: pre-release, saves are disposable (Cap7n 2026-08-28). Old profiles' placed lucky_coins may simply vanish.
- The meta tree's `lucky_coin` upgrade node gets repurposed or removed in the tool.

Related: [Ultimates & Spellbook](ultimates.md), [Artifacts & Relics](artifacts.md), [Economy & Gold](../game/economy.md).
