# ⚡ The Revamp (temporary tab)

**Tutorial slows down. Main game speeds up and hits players with meaningful choices, fast.**

Born from dev-to-dev playtest feedback (2026-07-19): players spend their attention decoding systems instead of fighting; choices don't land with the impact the audience expects; nobody realizes they can upgrade. The reference point we keep coming back to: **Vampire Survivors** — one character, and within the first 20 seconds you've killed things and been handed a 3-way build choice. In-your-face. That's the energy the main game needs. The gentle easing-in moves to a dedicated Tutorial.

This is a **two steps back, five steps forward** operation, so it gets its own tab while it's live. When both tracks are done, fold the results into the [Backlog](backlog.md) / [Decision Log](decisions.md) and delete this page.

---

## Track 1 — The Tutorial build (slow, its own thing)

The guided intro we already built becomes a dedicated MODE, and the main game gets stripped clean.

- [x] **1. Duplicate the level**: copy `level_1.tscn` → `tutorial_level.tscn`, near enough 1:1 — we already have most tutorial bits (TutorialDirector beats, rock-first gate, shop onboarding) and they simply move house.
- [x] **2. Start-menu "Tutorial" button** → launches the tutorial level with the guided intro FORCED on (a `Globals.tutorial_run` flag the TutorialDirector reads).
- [x] **3. Strip the main game**: `FORCE_EACH_RUN = false`; TutorialDirector goes fully dormant outside tutorial runs. Decide per beat: does it die in the main game or move to the tutorial? (The rock-first element gate is the big one — in the main game it likely DIES in favour of the opening element pick below.)
- [ ] **4. Slow the tutorial down** — the full curriculum below, one lesson block per wave, target **10–15 minutes total**. No game-over pressure (tower can't die, or respawns free).
- [ ] **5. Completion flag**: `tutorial done` saved to disk; first launch nudges new players toward the Tutorial button (but never forces it).

### The tutorial curriculum (target 10–15 min)

Everything a player must be SHOWN, grouped into wave-sized lessons. Ticks = wired into the tutorial level.

**Block 0 — Grace period (~1 min): you and the cart**

- [ ] 1. **Movement**: ride the rail with A/D.
- [ ] 2. **Camera tilt**: hold S to look down the wall (the beat exists in the director already — keep it here).

**Block 1 — Wave 1 (~2 min): fight**

- [ ] 3. **Dropping materials**: tap Space = one item, hold = pour the bin.
- [ ] 4. **Inventory selection**: scroll wheel picks which item drops next (the red-rimmed slot).
- [ ] 5. **Enemy telegraph**: the flare + the windrose point at where the wave comes from — get your cart to that side.
- [ ] 6. **Dealing with enemies**: rocks on heads; climbers get knocked off the wall (fall damage is your friend).
- [ ] 7. **Gold**: kills drop coins that arc to the pile on the tower top — the pile IS your bank.

**Block 2 — Rest 1 (~2 min): spending**

- [ ] 8. **The wave rhythm**: rest = shopping time; Enter calls the next wave early; the Auto next checkbox exists.
- [ ] 9. **TAB shop**: tower & cart upgrades (buy one, feel it), and the Wizards button bridges to the wizard shops.
- [ ] 10. **Wizard shop**: W or click a wizard; pick your element (rock-first gate teaches the flow here); buy Bigger Rock; the profile cards + Stats page.

**Block 3 — Wave 2 + Rest 2 (~3 min): defense**

- [ ] 11. **Brick armour**: let a few spiders dig in — bricks tint, crack and tumble; the wall soaks damage before your HP bar.
- [ ] 12. **Repair tool**: R or the button, click the damaged bricks, 1 gold per HP.
- [ ] 13. **Aimed throw**: right-click lobs the selected item at the ground, left-click at a hovered enemy (60° arc).

**Block 4 — Wave 3 + Rest 3 (~3 min): the toys**

- [ ] 14. **Artifacts**: grant one mid-wave — read the tooltip in the ✦ bag, place it on the tower, move it with the mover, double-click back to the bag. Mention the brick-break risk (wall = close to the action, top = safe).
- [ ] 15. **Second wizard**: hire from the TAB shop once gold allows; different wizards can take different elements (respec refunds 75%).

**Block 5 — Wave 4 (~2 min): graduation**

- [ ] 16. **Ultimates**: buy the ultimate node in a tree, watch it get scribed into the Spellbook, cast it.
- [ ] 17. **Enemy diversity**: an immune/resistant enemy shows up telegraphed — resistance icons, why one damage type stops working, answer = switch what you throw.
- [ ] 18. **Combo teaser**: one scripted moment of Frost then Rock (the brittle oneshot) — "elements combine; go find the rest."
- [ ] 19. **Wrap-up card**: "waves keep coming and grow endless, rest is for spending, good luck" → continue playing or return to menu.

**Deliberately NOT taught** (discovery is the fun): deep combos (oil+fire, lure stacking), enemy specials (pillbug charge, snail shells), trap-family artifacts, the balance-note key.

## Track 2 — The main game (all out, asap)

Attention spans are short. No easing in. The first minute must jingle the car keys.

- [ ] **1. Kill the slow open**: shorten the grace period; wave 1 spawns within seconds. Small wave, instant action.
- [ ] **2. The opening choice (the VS moment)**: after the first handful of kills (~20-30 seconds in), the game PRESENTS a **3-card element choice** — pick your starting wizard's element, free, right now. Replaces the rock-first gate in the main game. This is the build-defining "here's who you are this run" moment.
- [ ] **3. End-of-wave-1 relic choice**: upgrade the fixed Coin Maker grant into a **pick 1 of 3 artifacts** card moment. Same pattern, second hit of build identity. (The grant hook + bag already exist; this is a 3-card UI on top.)
- [ ] **4. Telegraph upgrades loudly**: flashing/glowing shop entry points when gold sits unspent, arrows/highlights on first-time surfaces, a rest-phase nudge line ("Spend your gold: TAB for tower, W for wizards"). Flash until first use, subtle after.
- [ ] **5. Make early choices HIT**: first upgrade tiers cheap and dramatic (big visible deltas, not +1 increments) so every pick visibly changes the run. Re-tune waves 1–5 for pressure — they were paced around the old tutorial drip.
- [ ] **6. Re-check the system reveal order**: with the tutorial gone, decide what the first 3 waves EXPOSE (spellbook already hides unearned spells; artifacts arrive end of wave 1 as the choice moment; TAB tree = telegraphed by the gold nudge). Rule of thumb: a player who ignores a system should still feel good for three waves.

## Open decisions along the way

- Does the opening 3-card choice offer 3 random elements of the 6, or a fixed starter trio (Rock always included)?
- Does the relic choice repeat (every N waves = a recurring VS-style draft?) or stay a wave-1 moment? A recurring draft could become THE meta-loop.
- What happens to the rock-first gate philosophy (cheap reliable baseline) — folded into the card copy ("Rock: the reliable one")?
- Wave 1 tuning: how many enemies before the element choice interrupts?

## Status log

- **2026-07-19** — page created; feedback recorded in the [Backlog](backlog.md) (fellow-dev section).
- **2026-07-20 (1 AM)** — Track 1 steps 1+2 DONE: tutorial_level.tscn duplicated (1:1, own uid pending editor save) + a Tutorial button on the start menu under Play (code-built, sets Globals.tutorial_run and loads the tutorial level). The TutorialDirector still runs in BOTH modes until step 3 strips the main game.
- **2026-07-20 (1:30 AM)** — Track 1 step 3 DONE: the guided intro runs ONLY in tutorial runs (Globals.tutorial_run gates _maybe_begin); the ROCK-FIRST ELEMENT GATE is tutorial-only too — the main game opens all six elements immediately (the interim all-out state until Track 2's 3-card opening pick). Telegraphs (flare, windrose, conjure circles, W-reach hint, repair labels) untouched — they are gameplay, not tutorial.
