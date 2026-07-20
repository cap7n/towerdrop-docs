# Ultimates & the Spellbook

The **Spellbook** (`UltimateBook` autoload) is an actual open **book**: a two-page spread with **element ribbon tabs** along its bottom edge, opened instantly from a right-edge pull tab. It is deliberately **non-modal** — no backdrop dim, the game keeps running visibly behind it — and tab switches are instant (the page-flip animation waits for the artist's real book art; the current parchment/leather look is a code-built placeholder).

## Gating: spells are earned in the trees (live since 2026-07-19)

A spell appears in the book **only after a wizard of that element buys its `ultimate_*` node in their upgrade tree**. Unowned spells are **fully hidden** (showing everything up front read as overwhelming — Cap7n call); an empty page says *"Nothing scribed yet. Ultimates learned in the wizard trees appear here."* Spells whose node exists but whose effect code isn't built yet show as "(spell not built yet)".

A **respec** (switching a wizard's element) can re-lock its spells; the book handles that live.

!!! note "Onboarding gap"
    Because unowned spells are hidden, nothing currently teaches players the book exists. A tutorial beat on the first ultimate purchase is a backlog item.

## The spells

Casts cost **15 gold, 10 seconds** each (flat placeholders to balance later).

| Spell | Element (tree node) | Effect | Status |
|---|---|---|---|
| **Oil Slick** | Rock (`ultimate_oil`) | Coats the wall; enemies slow into "bubble mode." **Ignites** into a fire wave when hit by fire (`ignite_world`). | ✅ Castable |
| **Meteor Shower** | Rock (`ultimate_meteors`) | Rains real rock items over the incoming sector for 10 s (`MeteorShower`, drip-spawned, fog-capped band, perf-capped). Rocks **crumble** on impact (`fx_crumble`). | ✅ Castable |
| **Deep Freeze** | Frost (`ultimate_freeze`) | Ice-rock crust climbs the tower; frozen enemies stop and take bonus damage from Rock/Electric/falls. | ✅ Castable |
| **Slime Coat** | Poison (`ultimate_slimey_walls`) | Jiggly mucus coat that **slows + poisons** climbers. | ✅ Castable |
| **FireWave** | Fire (`ultimate_firewave`) | The tower torches everything on it. | 📋 Effect not built |
| **Arc Storm** | Electric (`ultimate_arc`) | The tower arcs lightning from itself. | 📋 Effect not built |
| **Black-hole Lure** | Lure (`black_hole_lure`) | Yanks the whole sector into one pile. | 📋 Effect not built |

The three coats all run through `FluidField`, and reach **dug-in attackers** at the rim, not just surface climbers (`_apply_coat_effects`). Spikes are planned to fold in as another Rock spell (cast-for-gold with a duration).

!!! warning "Planned: granting-wizard bonuses"
    Decided but not implemented: a cast inherits the tree bonuses of the **wizard whose node added it to the book** (a rock wizard's rock upgrades apply to every meteor). This is what makes duplicate wizards taking different paths matter. See the [Decision Log](../project/decisions.md).

## Shaders

- `ice.gdshader`: fresnel-rim ice crust. (Planned extension: ice wall-coat + steam via a `FrostFx`.)
- Slime uses noise-blob geometry with a wobble driven by `TIME` and a wet rim.
- `fx_crumble.tscn`: the reusable no-glow debris burst (chunks + dust) rocks leave on impact.

## Related

- [The Tower & Base Defenses](../game/tower.md): what's being coated.
- [Atmosphere & VFX](atmosphere.md): the oil fluid system and fire that ignites it.
- [Items & Elements](../game/items.md): the per-element trees the ultimates are bought in.
