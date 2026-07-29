# Progression — growing every run

*Written 2026-07-25 after the "is this the same as Vampire Survivors?" conversation. Status: design page — the achievements/unlock layer exists; shards and recipes do not yet.*

## The diagnosis

The game has an open build space (elements, trees, artifacts, the economy-vs-damage axis) but **no structure around it**: nothing tells the player whether their build is working, losing banks nothing but achievements, and a run can see everything the game has. Result: "1 run and done."

Vampire Survivors has the **same open space and no exact path to victory either** — what it has is three kinds of structure. That's the checklist:

| Structure | VS | TowerDrop today | The plan |
|---|---|---|---|
| **A wall to calibrate against** | Death at minute 30 | Nothing — the snowball just wins | **[Bosses](../game/bosses.md)** every ~5 waves. "Is my build good?" acquires an answer: it beats the Queen or it doesn't |
| **Permanent power between runs** | The gold shop (+might, +projectile, forever) | Unlocks only — more *variety*, not more *power*. A player who loses tonight is exactly as strong tomorrow | **Shards**: bosses drop them (leading candidate for the parked earn-source question), a small shop spends them on permanent upgrades. Losing still feels like a deposit |
| **Named, discoverable goals** | Evolution recipes (Whip + Hollow Heart = Bloody Tear) | Combos exist (Oil+Fire ignite, Frost+Rock shatter) but are passive interactions, not goals | **Surface combos as named recipes in the Compendium** — cheap, and a run gains direction the moment the player picks one to chase |

## The economy axis is a feature — the wall makes it a bet

Coin Maker / Lucky Coin vs pure damage is the same axis as VS's beloved greed builds (Stone Mask). Today it's a *preference*, because nothing punishes greeding forever. With a boss at wave 10 it becomes a **bet with a deadline**: coin-make waves 1-6 and convert to damage in time, or go damage early and arrive poor but ready. The boss retroactively gives the economy artifacts their meaning. Don't nerf the greed path — give it a deadline.

## The three layers, named

1. **In-run**: the build (trees, TAB, draft, economy axis). Exists, needs tuning (0.12/0.13).
2. **Unlocks** (variety): achievements → artifacts/ultimates for the NEXT run's pool. Built 2026-07-25, hungry for content — bosses are the content.
3. **Permanent power** (strength): shards → small permanent upgrades. NOT built; the missing VS layer. Keep the numbers small (VS-style +5% steps) so skill still dominates.

## Design cautions

- **Do not build "the exact path."** Roguelites die the day they're solved. Several viable paths + sharp feedback (the wall) beats one authored path.
- **The wall must be legible**: the Queen's rock-resistant shield tells the player *why* they lost (all rock, no elements) — every boss should encode its lesson in its resistances/mechanics, so a loss reads as information, not punishment.
- Unlock rewards should widen *options*, shard purchases should deepen *power* — keep the two currencies philosophically separate or neither means anything.

## Related

- [Bosses & The Run Arc](../game/bosses.md) — the wall itself
- [Artifacts](artifacts.md) — the draft + unlock gating
- Backlog → Meta-progression: the tracker and routes that carry all of this
