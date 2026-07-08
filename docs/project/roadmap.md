# Roadmap & Priorities

The through-line. For the granular task list, see the [Backlog](backlog.md).

## Priority order

1. **Base-game fun and feel first.** The moment-to-moment loop — aiming from the cart, impacts, the scramble to the right arc of the rail — is the priority. Progression/economy cleanup is parked, not rejected.
2. **A stable, understandable playtest build (v0.1)** that's beatable to ~wave 10. This is the current organizing goal (see the [Backlog](backlog.md) tiers).

## The near-term path to a playtest build

In rough order of leverage:

1. **Item → effect coverage + the elements tuning pass.** Every buyable/usable item must do something; the six element trees need their first real tuning pass. This is the biggest "the game is coherent" win.
2. **Waves beatable to ≥10.** Tune the HP ramp and make sure the defenses keep up — the generator work (HP cap, powerups-not-HP) is the backbone here.
3. **Onboarding.** A tutorial that walks the *actual* systems (cart drop/pour/throw, element trees, shops, ultimates) plus a controls reference card. The current guided intro is a stopgap.
4. **Game-shell gaps.** Win/summary state at wave 10, clean restart, settings (volume + fullscreen).
5. **Looks-finished polish.** Real wizard models (in progress), HUD readability, key SFX.
6. **Perf Phase 0 quick wins** before a wider playtest so wave 10 doesn't chug — shadows, the allocation flood, ragdoll caps. See [Performance](../tech/performance.md).

## Bigger bets (after v0.1)

- **Tower-damage dig-in rework + visible tower damage** — enemies chewing a specific spot, with the tower visibly degrading. The brick-shell damage-state system is the foundation. See [The Tower](../game/tower.md).
- **More enemy types** via the component system (termite, bombardier, carrier, centipede), then delete the legacy inline enemy path.
- **Elemental combos** — the set-up/pay-off pattern that gives every element a reason to exist. Start with the nearly-free ones (Oiled+Fire done, Electric+Oil next).
- **Buildable defenses & traps** — buy-then-throw into place.

!!! note
    This roadmap is the *direction*; the [Backlog](backlog.md) is the *inventory*. When priorities shift, update this page's ordering — it's the fastest way to see what "now" means.
